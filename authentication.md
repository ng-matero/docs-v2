# 登录认证

> 这里采用 `JWT` 作为登录方式，假设你的 Api 和 Web 地址分离。
## 1. 环境变量和依赖注入

这里的环境变量和 [官方文档](https://angular.cn/guide/build) 差不多，不过**默认的情况下为开发环境**。

那么和往常一样，将接口地址写进 `environment` 属性当中。

```diff
export const environment = {
  production: false,
  baseUrl: '',
  useHash: false,
+ apiUrl: 'http://my-api-url',
};
```

**我们推荐创建一个 HTTP 拦截器，类似于 `BASE_URL` 一样使用 `InjectionToken` 。**

```bash
$ ng g interceptor core/interceptors/apiUrl --functional false
```

```ts
import { inject, Injectable, InjectionToken } from '@angular/core';
import { HttpRequest, HttpHandler, HttpEvent, HttpInterceptor } from '@angular/common/http';
import { Observable } from 'rxjs';

export const API_URL = new InjectionToken<string>('API_URL');

@Injectable()
export class ApiUrlInterceptor implements HttpInterceptor {
  private readonly apiUrl = inject(API_URL, { optional: true });

  private hasScheme = (url: string) => this.apiUrl && new RegExp('^' + this.apiUrl, 'i').test(url);

  intercept(request: HttpRequest<unknown>, next: HttpHandler): Observable<HttpEvent<unknown>> {
    return this.hasScheme(request.url) === true
      ? next.handle(request.clone({ withCredentials: false })) // If your api does not support cookie when CORS,set the attribute withCredentials to false.
      : next.handle(request);
  }
}
```

然后将拦截器统一导出。

```diff
// index.ts
...
+ export * from './api-url.interceptor';

/** Http interceptor providers in outside-in order */
export const httpInterceptorProviders = [
  { provide: HTTP_INTERCEPTORS, useClass: NoopInterceptor, multi: true },
  // { provide: HTTP_INTERCEPTORS, useClass: SanctumInterceptor, multi: true },
  { provide: HTTP_INTERCEPTORS, useClass: BaseUrlInterceptor, multi: true },
  { provide: HTTP_INTERCEPTORS, useClass: SettingsInterceptor, multi: true },
  { provide: HTTP_INTERCEPTORS, useClass: TokenInterceptor, multi: true },
  { provide: HTTP_INTERCEPTORS, useClass: DefaultInterceptor, multi: true },
  { provide: HTTP_INTERCEPTORS, useClass: ErrorInterceptor, multi: true },
  { provide: HTTP_INTERCEPTORS, useClass: LoggingInterceptor, multi: true },
+ { provide: HTTP_INTERCEPTORS, useClass: ApiUrlInterceptor, multi: true },
];
```

**接下来就是依赖注入了。**

```diff
// app.config.ts
export const appConfig: ApplicationConfig = {
  providers: [
    ...
    { provide: BASE_URL, useValue: environment.baseUrl },
+   { provide: API_URL, useValue: environment.apiUrl }, 
	...
  ]
}
```

## 2. 登录服务

这一步当中，删除 `FakeLoginService` ，同时依赖注入使用 `LoginService` 作为值。

> `FakeLoginService` 的目的是为了演示，不应该在实际应用中使用。

```diff
// app.config.ts
export const appConfig: ApplicationConfig = {
  providers: [
    ...
-	{ provide: LoginService, useClass: FakeLoginService },
+   { provide: LoginService, useClass: LoginService },
    ...
  ]
}
```

然后，登录服务网络请求地址进行修改。

```ts
import { HttpClient } from '@angular/common/http';
import { Injectable, inject } from '@angular/core';
import { map } from 'rxjs';

import { API_URL, Menu } from '@core';
import { Token, User } from './interface';

@Injectable({
  providedIn: 'root',
})
export class LoginService {
  protected readonly http = inject(HttpClient);
  protected readonly apiUrl = inject(API_URL);

  login(username: string, password: string, rememberMe = false) {
    return this.http.post<Token>(`${this.apiUrl}/auth/login`, { username, password, rememberMe });
  }

  refresh(params: Record<string, any>) {
    return this.http.post<Token>(`${this.apiUrl}/auth/refresh`, params);
  }

  logout() {
    return this.http.post<any>(`${this.apiUrl}/auth/logout`, {});
  }

  me() {
    return this.http.get<User>(`${this.apiUrl}/me`);
  }

  menu() {
    return this.http.get<{ menu: Menu[] }>(`${this.apiUrl}/me/menu`).pipe(map(res => res.menu));
  }
}
```

> 可以根据实际情况，将请求返回类型进行修改，比如接口带有信息的封装。

## 3. 请求头携带Token

但是，此时会发现，登录成功后又跳转回了登录页面。

因为进入主页会调用 `me` 、`menu` 方法，但请求没有携带 token。

**而返回的 401 错误在 `ErrorInterceptor` 拦截器会跳转回登录页面。**

**携带 token 的处理部分在 `TokenInterceptor` 拦截器中**。默认的情况下，`baseUrl` 视为是在同一域下。（也就是服务器部署 api 和 web 的生产地址前缀相同）

> 将 `baseUrl` 处理的部分，复制更改成 `apiUrl` 即可

```diff
// token-interceptor.ts
@Injectable()
export class TokenInterceptor implements HttpInterceptor {
  private readonly router = inject(Router);
  private readonly tokenService = inject(TokenService);
  private readonly baseUrl = inject(BASE_URL, { optional: true });
+ private readonly apiUrl = inject(API_URL, { optional: true });

  ...

  private shouldAppendToken(url: string) {
+   return !this.hasHttpScheme(url) || this.includeBaseUrl(url) || this.includeApiUrl(url);
  }

  private includeBaseUrl(url: string) {
    if (!this.baseUrl) {
      return false;
    }

    const baseUrl = this.baseUrl.replace(/\/$/, '');

    return new RegExp(`^${baseUrl}`, 'i').test(url);
  }

+ private includeApiUrl(url: string) {
+   if (!this.apiUrl) {
+     return false;
+   }
+
+  const apiUrl = this.apiUrl.replace(/\/$/, '');
+
+   return new RegExp(`^${apiUrl}`, 'i').test(url);
+ }
}
```

这样就可以在请求头的 `Authorization` 字段当中看到携带的 token 。
