# Authentication

## 1. Delete FakeLoginService

**The `FakeLoginService` is only for demonstrating the effect before and after login and should not exist in a real-world project.**

Remove the `src\app\fake-login.service.ts` file.

Next, make some modifications to the application configuration.

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

## 2. Using a Local Proxy

We recommend accessing the backend API endpoints through a [proxy](https://angular.dev/tools/cli/serve/#proxying-to-a-backend-server) in a development environment, while using tools like nginx for deployment in production environments.

```diff
// proxy.config.js
const PROXY_CONFIG = {
  ...
+ '/api': {
+   target: 'http://my-api-url',
+   secure: false,
+   pathRewrite: {
+     '^/api': '',
+   },
+   changeOrigin: true,
+ },
};

module.exports = PROXY_CONFIG;
```

## 3. Login Service

After that, the network request address for the login service needs to be modified.

```typescript
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

> Based on the actual situation, you can modify the return type of the request, such as encapsulating the response data with additional information.

## SP. Nginx Deployment

Here is a simplified introduction to the configuration method for nginx, which mainly involves rewriting the path for the API endpoints, consistent with typical reverse proxy setups.

```nginx
server {
    listen 80;
    server_name www.example.com;
	...
    location / {
        # ng-matero web server
        proxy_pass http://localhost:port;
        ...
    }
    location /api {
        # ng-matero api server
        rewrite ^/api/?(.*)$ /$1 break; # If the actual path does not have an api prefix
        proxy_pass http://localhost:port;
        ...
    }
}
```
