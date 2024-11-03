+++
title = 'Guard Route'
date = 2024-09-01T08:51:00+07:00
draft = false
weight = 3
pre = '4.2.3 '
+++
**Bước 1:** chạy command sau: 
```
ng g g canActivate
ng g s auth

```
![authService path](/images/4.buildApp/4.2-frontend/pic3.png)
**Bước 2:** mở authService vừa tạo ra, triển khai hàm isAuthenticated như sau: 
```typescript
import { Injectable } from '@angular/core';
import { getCurrentUser } from 'aws-amplify/auth';

@Injectable({
  providedIn: 'root'
})
export class AuthService {

  constructor() { }

  async isAuthenticated() {
    try {
      await getCurrentUser()
      return true
    } catch (error) {
      return false;
    }
  }
}
```
![guard path](/images/4.buildApp/4.2-frontend/pic4.png)
**Bước 3:** mở can-activateGuard vừa tạo ra, copy đoạn code dưới đây vào:
```typescript
import { inject } from '@angular/core';
import { CanActivateFn, Router } from '@angular/router';
import { AuthService } from './auth.service';

export const canActivateGuard: CanActivateFn = async () => {
  const router = inject(Router)
  const auth = inject(AuthService)

  if (await auth.isAuthenticated())
    return true;
  router.navigate(['login'])
  return false
};
```
{{% notice note%}}
Như vậy, route home sẽ chỉ có thể được truy cập nếu đã được authenticated, nếu không sẽ bị redirect tới route login
{{% /notice %}}

Tiếp theo, chúng ta sẽ học cách triển khai các chức năng crud cơ bản trên trang web này.


