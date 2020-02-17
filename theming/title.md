# Page Title

The page title by default gets the `name` of the menu

```html
<page-header></page-header>
```

You can use the [color helpers](helpers.md) directly to change the background color of the page title.

```html
<page-header class="bg-pink-A100"></page-header>
```

The page title component is made up of the page title and the breadcrumbs. You can also use the breadcrumbs component directly.

```html
<breadcrumb></breadcrumb>
```

## API

### page-header


| Property         | Description        | Type      | Default  |
|------------------|--------------------|-----------|----------|
| `title`          | title              | `string`  | `''`     |
| `subtitle`       | subtitle           | `string`  | `''`     |
| `showBreadCrumb` | if show breadcrumb | `boolean` | `true`   |


### breadcrumb

| Property         | Description  | Type      | Default  |
|------------------|--------------|-----------|----------|
| -                | -            | -         | -        |