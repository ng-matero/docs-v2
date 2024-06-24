# Permissions

The ng-matero uses [ngx-permissions](https://github.com/AlexKhymenko/ngx-permissions) to manage permissions. Please check the [docs](https://github.com/AlexKhymenko/ngx-permissions/wiki) for more details.

### Menu Permissions

The ngx-permissions directive is already built into the ng-matero's menu. You can add the `permissions` field directly from the menu's configuration.

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
