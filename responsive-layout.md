# 响应式布局

Angular Material 默认没有响应式布局组件，我们必须借助 Angular 官方提供的 flex-layout 实现响应式布局。

flex-layout 是一个基于指令的布局神器，专为 Angular 设计，使用非常方便。

本人在使用中发现了一些小问题，列之间的间距需要使用 `fxLayoutGap` 指令实现，因为一些说起来有些复杂的原因，这个指令并不是很好用，已经有人提议加入 `fxLayoutGutter` 指令，但是官方目前因为各种原因没有实现。

为了解决这个问题，ng-matero 暂时加入了 `matero-row` `matero-col` 两个类实现左右间距，以后有可能会移除。使用者可以看一下完整 DEMO 的 dashboard 代码。
