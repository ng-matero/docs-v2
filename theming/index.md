# 主题系统

Ng-Matero 的主题系统提供了两种常用布局（`侧边栏导航布局` 及 `顶部导航布局`）、菜单服务、页面标题组件（面包屑组件）、常用辅助样式类等。

> 建议在项目初始化的时候选择一个固定的布局类型，尽量不要在项目启动后通过 `options` 去设置布局，除非你有特殊的需求。

Ng-Matero 的主题样式是基于 Sass 编写，所以使用者必须了解 Sass 的基础知识。主题样式目录如下：

```plain
├── styles                                  # 样式目录
│   ├── functions                           # 函数
│   ├── helpers                             # 工具类
│   ├── mixins                              # mixin
│   ├── plugins                             # 第三方库样式
│   ├── theme                               # 主题核心样式
│   ├── widgets                             # 公用组件
│   ├── _functions.scss                     # 函数入口
│   ├── _mixins.scss                        # mixin 入口
│   ├── _variables.scss                     # 变量
└── └── app.scss                            # 主题样式入口文件
```

为了方便修改主题色系，`sidenav` 和 `topnav` 使用了 mixin。

```scss
@mixin matero-admin-theme($theme) {
  @include matero-sidenav-theme($theme);
  @include matero-topnav-theme($theme);
}
```

![](theme.jpg)