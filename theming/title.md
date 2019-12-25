# 页面标题

使用页面标题组件。页面标题默认获取菜单的 `name`

```html
<page-header></page-header>
```

可以直接使用 [颜色类](helpers.md) 来改变标题的背景色。

```html
<page-header class="bg-pink-A100"></page-header>
```

页面标题组件是由页面标题及面包屑组成，也可以直接使用面包屑组件。

```html
<breadcrumb></breadcrumb>
```

## API

### page-header


| 属性              | 说明         | 类型       | 默认值    |
|------------------|--------------|-----------|----------|
| `title`          | 标题          | `string`  | `''`     |
| `subtitle`       | 副标题        | `string`   | `''`     |
| `showBreadCrumb` | 是否显示面包屑 | `boolean`  | `true`   |


### breadcrumb

| 属性              | 说明         | 类型       | 默认值    |
|------------------|--------------|-----------|----------|
| -                | -            | -         | -        |