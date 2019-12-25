# Add a New Route

Ng-Matero currently provides two schematics for generating codes, `ng g ng-matero:module` and `ng g ng-matero:page`. These two commands are extentions to Angular CLI `ng g module` and `ng g component`.

## Module

Create a lazy module by default.

```bash
$ ng g ng-matero:module <module-name>
```

The new module will be created in `routes` file, it will be added in `routes.module` and its route declaration will be added in `routes-routing.module` automaticly:

```diff
const routes: Routes = [
  {
    path: '',
    component: AdminLayoutComponent,
    children: [
+     { path: 'abc', loadChildren: () => import('./abc/abc.module').then(m => m.AbcModule) },
    ],
  }
]
```

The route declarations will be added in children route of `AdminLayoutComponent` by default and no other insertion location can be set for the time being.

### Vs origin module

Ng-Matero's module is equivalent to the following `ng` CLI：

```bash
$ ng g module routes/abc -m=routes --route=abc
```

> The Angular CLI 8.x version has a bug, it will throw an error with above command. I have submit an issue and pull a request. The Angular team has optimized for the problem, but is not sure which release fixes it, so check for yourself.

But when Angular generates a lazy module, it declares a route to the sibling route location:

```diff
const routes: Routes = [
  {
    path: '',
    component: AdminLayoutComponent,
    children: [],
  },
+ { path: 'abc', loadChildren: () => import('./abc/abc.module').then(m => m.AbcModule) },
]
```

## Page

Create a business page in the lazy module.

```bash
$ ng g ng-matero:page <page-name> -m=<module-name>
```

The above command will add a component declaration in the new lazy module and a routing declaration in the lazy module route.

### Dynamic Component

Dynamic components are those that need to be added to `entryComponents` in short. These components are often used for modal, such as `MatBottomSheet` and `MatDialog` in Angular Material.

Add a dynamic component to the business page.

```bash
$ ng g ng-matero:page <page-name>/<entry-component-name> -m=<module-name> -e=true
```

After the creation is complete, you need to manually write the component content, which is often used for editing modal. Corresponding modal templates schematic may be added in subsequent iterations.

### Vs origin component

Using the original `component` can only create components, not add routing declarations. For example, the following methods can only add a component in a lazy module, the other declarations need to be added manually.

```bash
$ ng g component routes/<module-name>/<page-name> -m=routes/<module-name>
```

The main differences between `ng-matero:page` and `component` are as follows:

* Optimize component name: to prevent component duplication, add the module name prefix by default.
* Optimize component generation path: add to `routes` by default.
* Add declarations to constant array, e.g. `COMPONENTS`.
* Add the component's routing declaration to the module routing.

