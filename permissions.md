# 权限管理

ng-matero 使用 [ngx-permissions](https://github.com/AlexKhymenko/ngx-permissions) 进行权限管理。具体用法可以查阅 ngx-permissions 的[文档](https://github.com/AlexKhymenko/ngx-permissions/wiki)。

### 菜单权限

在 ng-matero 的菜单中已经内置了 ngx-permissions 的指令。可以在菜单的配置项中直接添加 `permissions` 字段。

```diff
{
  "menu": [
    ...
    {
      "route": "design",
      "name": "design",
      "type": "sub",
      "icon": "color_lens",
+     "permissions": {
+       "only": [
+         "ADMIN",
+         "MANAGER"
+       ]
+     }
    },
    ...
  ]
}
```

[https://ng-matero.github.io/ng-matero/permissions/role-switching](https://ng-matero.github.io/ng-matero/permissions/role-switching)
