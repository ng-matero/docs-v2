# Flex layout

[flex-layout](https://github.com/angular/flex-layout) layout engine based on directives designed for Angular. It's easy to use.

More details to flex-layout [wiki](https://github.com/angular/flex-layout/wiki).

#### Issues

I found some problems when using it. The spacing between columns needs to be implemented by `fxLayoutGap` instruction. This directive doesn't work very well for some complicated reasons. Someone been suggested to add the `fxLayoutGutter` directive, but it has not been implemented officially for various reasons.

To solve this problem, ng-matero has temporarily added two classes, `matero-row` and `matero-col` to achieve columns spacing, which may be removed in the future. Users can see the dashboard code of the full DEMO.

