# 样式辅助类

CSS 辅助类在实际工作中有着至关重要的作用，基于 Angular Material 组件库以及 CSS 辅助类，我们几乎不需要再写多余的样式，同时也可以避免出现全局样式覆盖的问题或者这一隐患降到最低，系统中所有的细节调整都应该使用 helpers。

我在很早之前曾写过 CSS 辅助类的编写策略，详见 [如何编写通用的 Helper Class](https://www.cnblogs.com/nzbin/p/7746047.html)。

Ng-Matero 中辅助样式类遵循 Material 的设计原则，数值大多是 4 的倍数，比如 Padding 的辅助类 `p-4` `p-8` `p-16` `p-24`，这样可以最大程度的保证系统样式的统一。

## 关于颜色

颜色类分为背景色及文本色，背景色添加 `.bg-` 前缀，文本色添加 `.text-` 前缀

## 可用的 CSS 辅助类

所有 CSS 辅助类可以查看 [Ng-Matero Helpers](https://ng-matero.github.io/ng-matero/#/helpers)

