# 登录认证

### 1. 删除FakeLoginService

**`FakeLoginService`只是为了演示登录前后的效果，不应该在实际项目当中存在。**

将`src\app\fake-login.service.ts`文件删除。

接下来修改一下_应用配置_。

```diff
// app.config.ts
-import { FakeLoginService } from './fake-login.service';
...
export const appConfig: ApplicationConfig = {
  providers: [
    ...
-   // ==================================================
-   // 👇 ❌ Remove it in the realworld application
-   //
-   { provide: LoginService, useClass: FakeLoginService },
+   { provide: LoginService, useClass: LoginService },
    ...
  ]
}
```

### 2. 使用本地代理

我们推荐将后端的接口地址，在_开发环境_通过[代理](https://angular.dev/tools/cli/serve#proxying-to-a-backend-server)进行访问，而_生产环境_中则是使用像 nginx 进行部署。

```diff
// proxy.config.js
const PROXY_CONFIG = {
  ...
+ '/api': {
+   target: 'http://my-api-url',
+   secure: false,
+   pathRewrite: {
+     '^/api': '', // 这样请求接口实际地址时，路径不会有 api 前缀
+   },
+   changeOrigin: true, // 如果对应接口地址不在 localhost 情况下
+ },
};

module.exports = PROXY_CONFIG;
```

> `ng-matero` 已经默认支持代理，所以不需要修改 `angular.json` 文件。

### 3. 登录服务

然后，登录服务网络请求地址进行修改。

```ts
import { HttpClient } from '@angular/common/http';
import { Injectable, inject } from '@angular/core';
import { map } from 'rxjs';

import { Menu } from '@core';
import { Token, User } from './interface';

@Injectable({
  providedIn: 'root',
})
export class LoginService {
  protected readonly http = inject(HttpClient);

  login(username: string, password: string, rememberMe = false) {
    return this.http.post<Token>('/api/auth/login', { username, password, rememberMe });
  }

  refresh(params: Record<string, any>) {
    return this.http.post<Token>('/api/auth/refresh', params);
  }

  logout() {
    return this.http.post<any>('/api/auth/logout', {});
  }

  me() {
    return this.http.get<User>('/api/me');
  }

  menu() {
    return this.http.get<{ menu: Menu[] }>('/api/me/menu').pipe(map(res => res.menu));
  }
}
```

> 可以根据实际情况，将请求返回类型进行修改，比如接口带有信息的封装。

### SP. Nginx 部署

这里介绍最简单化的配置方式，只需要将接口地址重写一下路径，和通常的反向代理一致。

```nginx
server {
    listen 80;
    server_name www.example.com;
	...
    location / {
        # ng-matero 项目 web 服务器
        proxy_pass http://localhost:port;
        ...
    }
    location /api {
        # ng-matero 项目 api 服务器
        rewrite ^/api/?(.*)$ /$1 break; # 如果真实路径没有 api 前缀
        proxy_pass http://localhost:port;
        ...
    }
}
```
