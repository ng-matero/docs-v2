# 新增路由

Ng-Matero 目前提供了两个生成代码的原理图，`ng g ng-matero:module` 及 `ng g ng-matero:page`，其实这两个命令是对 Angular CLI `ng g module` 和 `ng g component` 的扩展优化。

## 模块

默认创建一个惰性模块。

```bash
$ ng g ng-matero:module <module-name>
```

通过以上命令会自动在 `routes` 目录添加一个业务模块，同时也会将模块声明添加到 `routes.module` 以及在 `routes-routing.module` 中添加一个路由定义。比如：

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

路由定义默认添加到 `AdminLayoutComponent` 的子路由中，暂时无法设置其它插入位置。

### 对比原始 module

Ng-Matero 的 module 原理图等价于以下 `ng` CLI：

```bash
$ ng g module routes/abc -m=routes --route=abc
```

> Angular CLI 8.x 版本有 bug，使用以上命令会报错，本人提了 issue 及 pr，Angular 团队已经针对该问题做了优化，但是不确定在哪个版本修复了该问题，大家可以自行检查。

但是 Angular 生成惰性模块时，会将路由添加声明到同级路由位置，例如：

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

## 页面

在惰性模块中创建一个业务页面。

```bash
$ ng g ng-matero:page <page-name> -m=<module-name>
```

以上命令会在新建的惰性模块中添加组件声明，同时在惰性模块路由中添加一个路由声明。

### 动态组件

动态组件简单来说就是需要添加到 `entryComponents` 中去的组件，这一类组件常用于模态窗等，比如 Angular Material 中的 `MatBottomSheet`，`MatDialog` 等。

在业务页面添加一个动态组件。

```bash
$ ng g ng-matero:page <page-name>/<entry-component-name> -m=<module-name> -e=true
```

创建完成之后需要手动编写组件内容，常用于编辑操作的模态窗。在后续迭代中可能会增加相应的模态窗组件模板。

### 对比原始 component

使用原始的 `component` 只能创建组件，无法添加路由声明，比如以下方法只能在惰性模块中添加一个组件，其它声明需要手动添加。

```bash
$ ng g component routes/<module-name>/<page-name> -m=routes/<module-name>
```

`ng-matero:page` 和 `component` 的主要不同如下：

- 优化组件命名：为了防止组件重复，默认添加模块名前缀
- 优化组件生成路径：默认添加到 `routes`
- 将声明添加到常量数组，比如 `COMPONENTS`
- 在模块路由中添加组件的路由声明

