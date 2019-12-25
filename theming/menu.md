# 配置菜单

菜单服务会返回一个 Menu 数组，应用的导航会根据菜单数据动态生成，并且和路由相关联。每个菜单项的路由是由父级和子级的 `state` 组合而成。我们稍后用一个示例说明，以下是菜单的类型定义。

```ts
export interface Tag {
  color: string; // Background Color
  value: string;
}

export interface ChildrenItem {
  state: string;
  name: string;
  type: 'link' | 'sub' | 'extLink' | 'extTabLink';
  children?: ChildrenItem[];
}

export interface Menu {
  state: string;
  name: string;
  type: 'link' | 'sub' | 'extLink' | 'extTabLink';
  icon: string;
  label?: Tag;
  badge?: Tag;
  children?: ChildrenItem[];
}
```

## 路由路径

ng-matero 的菜单只支持三级，一般来说，二级菜单已经够用了。

以二级菜单为例：一级子项的 `state` 表示惰性模块的路由 `path`，二级子项的 `state` 表示业务组件的路由 `path`，所以业务页的最终路由地址就是 `level_1_state/level_2_state`。在少数情况下可能会用到三级菜单。

以下是一个三级菜单的示例。如果使用三级菜单，其二级子项的 `state` 允许为空，最终的路由地址为 `material/autocomplete`。但是这种情况下会有一个问题，路由地址无法关联到菜单的第三级。

```json
{
  "menu": [
    {
      "state": "material",
      "name": "Material",
      "type": "sub",
      "icon": "favorite",
      "children": [
        {
          "state": "",
          "name": "Form Controls",
          "type": "sub",
          "children": [
            {
              "state": "autocomplete",
              "name": "Autocomplete",
              "type": "link"
            }
          ]
        }
      ]
    }
  ]
}
```

关于三级菜单的问题可以看一下演示示例的路由地址，分别打开以下两个路径查看菜单变化：

- [https://ng-matero.github.io/ng-matero/#/material/autocomplete](https://ng-matero.github.io/ng-matero/#/material/autocomplete)
- [https://ng-matero.github.io/ng-matero/#/material/data-table/paginator](https://ng-matero.github.io/ng-matero/#/material/data-table/paginator)

## 标签颜色

标签颜色需要填入一个合法的 Material 颜色值，比如 `red-500`、`blue-900`，详情查看 [颜色](colors.md)

## API

### MenuService

| 方法             | 参数                 | 返回值    | 描述          |
|-----------------|----------------------|----------|--------------|
| getAll          | -                    | `Menu[]` | 获取全部菜单   |
| set             | `menu: Menu[]`       | `Menu[]` | 设置菜单      |
| add             | `menu: Menu`         | -        | 添加一个菜单项 |
| getMenuItemName | `stateArr: string[]` | `string` | 获取菜单项名称 |
| getMenuLevel    | `stateArr: string[]` | `string` | 获取菜单层级   |