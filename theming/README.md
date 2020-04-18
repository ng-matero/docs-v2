# Theming

Ng-Matero's theme system provides two commonly used layouts \(`side navigation` and `top navigation`\), menu services,page title component and breadcrumb component, commonly used css helpers, etc。

> It is recommended that you choose a fixed layout type during project initialization, and do not use `options` to set the layout after project startup unless you have special requirements.

Ng-Matero's theme styles are written based on Sass, so users must understand the basics of Sass. The theme style directory is as follows:

```text
├── styles
│   ├── functions
│   ├── helpers
│   ├── mixins
│   ├── plugins
│   ├── theme
│   ├── widgets
│   ├── _functions.scss
│   ├── _mixins.scss
│   ├── _variables.scss
└── └── app.scss
```

`sidenav` and `topnav` used mixins to make it easier to change the theme color.

```css
@mixin matero-admin-theme($theme) {
  @include matero-sidenav-theme($theme);
  @include matero-topnav-theme($theme);
}
```

![](../.gitbook/assets/theme.jpg)

