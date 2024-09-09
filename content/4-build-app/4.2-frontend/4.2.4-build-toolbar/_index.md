+++
title = 'Xây dựng Toolbar'
date = 2024-09-07T10:20:12+07:00
draft = false
weight = 4
pre = "4.2.4 "
+++
**Bước 1:** Thêm angular material vào project bằng cách chạy command sau từ root: 
```
npm install -g @angular/cli@17 
```
**Bước 2:** Chạy command sau để tạo toolbarComponent trong homeComponent:

```
ng g c home/toolbar 
```
Các option đều để mặc định.
**Bước 3:** Import hàm signOut từ "amplify/auth"
```
    ng g s auth
```

**Bước 4:** Trong auth.service.ts, implement 2 function signOut và getUser như sau: 
{{< tabs groupId="auth" >}}
    {{% tab name="./src/app/aut.service.ts" %}}
```typescript
/* ... */
import { AuthUser, getCurrentUser, signOut } from 'aws-amplify/auth';
/* ... */

export class AuthService {
/* ... */
  user: Partial<AuthUser> = {}

  async getUser() {
    if (JSON.stringify(this.user) == '{}')
      this.user = await getCurrentUser()

    return this.user;
  }
  async signOut() {
    this.user = {}
    await signOut()
    this.router.navigate(['login'])
  }
/* ... */
}
```
    {{% /tab %}}
{{< /tabs >}}
**Bước 5:**  Triển khai thành toolbar hoàn chỉnh có button signout sử dụng các designed component có sẵn của angular material:

{{< tabs groupId="toolbar" >}}
    {{% tab name="./src/app/home/toolbar/toolbar.component.ts" %}}
```typescript
import { Component } from '@angular/core';
import {MatIconModule} from '@angular/material/icon';
import {MatButtonModule} from '@angular/material/button';
import {MatToolbarModule} from '@angular/material/toolbar';
import { AuthService } from '../../auth.service';

@Component({
  selector: 'app-toolbar',
  standalone: true,
  imports: [
    MatButtonModule,
    MatToolbarModule,
    MatIconModule,
  ],
  templateUrl: './toolbar.component.html',
  styleUrl: './toolbar.component.css'
})
export class ToolbarComponent {
  constructor(private auth: AuthService) {}

  async signOut() {
    await this.auth.signOut()
  }
}
```
    {{% /tab %}}
    {{% tab name="src/app/home/toolbar/toolbar.component.html" %}}
``` html 
<p>
  <mat-toolbar color="accent">
    <button mat-icon-button class="example-icon" aria-label="Example icon-button with menu icon">
      <mat-icon>menu</mat-icon>
    </button>
    <span>My App</span>
    <span class="example-spacer"></span>
    <button mat-button class="example-icon" aria-label="Example button with sign out icon" (click)="signOut()">
      Sign out
    </button>
  </mat-toolbar>
</p>
```
    {{% /tab %}}
    {{% tab name="src/app/home/toolbar/toolbar.component.css" %}}
```
p {
  margin-bottom: 10px;
}
.example-spacer {
  flex: 1 1 auto;
}
```
    {{% /tab %}}
  {{< /tabs >}}

**Bước 6:** Cuối cùng, sử dụng toolbar component trong home component bằng cách: 
{{< tabs groupId="home" >}}
  {{% tab name="./src/app/home/home.component.html" %}}
```html
<app-toolbar></app-toolbar>
```
  {{% /tab %}}
  {{% tab name="./src/app/home/home.component.ts" %}}
```typescript

import { ToolbarComponent } from './toolbar/toolbar.component';
/* ... */

@Component({
  /* ... */
  imports: [
    ToolbarComponent
  ],
  /* ... */
})
/* ... */
```
  {{% /tab %}}
{{< /tabs >}}
Sau khi triển khai xong, toolbar của chúng ta sẽ trông như thế này:
![hell](/images/4.buildApp/4.2-frontend/pic5.png)