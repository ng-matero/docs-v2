# Layout Settings

The layout configuration is only valid when you choose `dynamic` template during project initialization.

`core/settings.ts` is the default value for the layout configurations. Where `navPos`,` dir `, `theme` are associated with the prebuilt options.

```ts
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

It's recommended not to change `setting.ts`, but to dynamically set the page layout by using` setLayout `in` setting.service.ts`.

## API

### SettingsService

| Method      | Parameter                       | Return            | Description                           |
|-------------|---------------------------------|-------------------|---------------------------------------|
| getOptions  | -                               | `AppSettings`     | Get configuration options             |
| setLayout   | `options?: AppSettings`         | `AppSettings`     | Set the layout                        |
| setNavState | `type: string` `value: boolean` | `Observable<any>` | Listens for sidebar navigation status |
