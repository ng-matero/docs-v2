# 配置布局

布局配置只有在项目初始化时选择 `dynamic` 模板才有效。

`core/settings.ts` 是布局配置项的默认值，其中 `navPos`、`dir`、`theme` 和预构建选项关联。

```typescript
export interface AppSettings {
  navPos?: 'side' | 'top';
  dir?: 'ltr' | 'rtl';
  theme?: 'light' | 'dark';
  showHeader?: boolean;
  headerPos?: 'fixed' | 'static' | 'above';
  showUserPanel?: boolean;
  sidenavOpened?: boolean;
  sidenavCollapsed?: boolean;
}

export const defaults: AppSettings = {
  navPos: 'side',
  dir: 'ltr',
  theme: 'light',
  showHeader: true,
  headerPos: 'fixed',
  showUserPanel: true,
  sidenavOpened: true,
  sidenavCollapsed: false,
};
```

建议不要改动 `setting.ts`，动态设置页面布局可以使用 `setting.service.ts` 服务中的 `setLayout`。

## API

### SettingsService

| 方法 | 参数 | 返回值 | 描述 |
| :--- | :--- | :--- | :--- |
| getOptions | - | `AppSettings` | 获取配置项 |
| setLayout | `options?: AppSettings` | `AppSettings` | 设置布局 |
| setNavState | `type: string` `value: boolean` | `Observable<any>` | 监听侧边栏导航状态 |

