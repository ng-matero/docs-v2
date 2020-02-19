# Flex Layout

[flex-layout](https://github.com/angular/flex-layout) 是一个专为 Angular 设计的基于指令的布局神器，使用非常方便。具体使用方式可以参见 flex-layout 的[文档](https://github.com/angular/flex-layout/wiki)。

### Issues

本人在使用中发现了一些小问题，列之间的间距需要使用 `fxLayoutGap` 指令实现，因为一些说起来有些复杂的原因，这个指令并不是很好用，已经有人提议加入 `fxLayoutGutter` 指令，但是官方目前因为各种原因没有实现。

为了解决这个问题，ng-matero 暂时加入了 `matero-row` `matero-col` 两个类实现左右间距，以后有可能会移除。使用者可以看一下完整 DEMO 的 dashboard 代码。

