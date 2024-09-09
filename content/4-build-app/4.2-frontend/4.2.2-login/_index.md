+++
title = 'Xây dựng phần login'
date = 2024-08-30T16:47:15+07:00 
draft = false
pre = '4.2.2 '
weight = 2
+++
![app.component.ts path](/images/4.buildApp/4.2-frontend/pic1.png)
**Bước 1:** kết nối amplify với frontend. Trong file src/app/app.component.ts, update thành đoạn code sau:  

```typescript
import { Component } from '@angular/core';
import { RouterOutlet } from '@angular/router';
import  outputs from "../../amplify_outputs.json";
import { Amplify } from 'aws-amplify';

Amplify.configure(outputs)
@Component({
  selector: 'app-root',
  standalone: true,
  imports: [RouterOutlet],
  templateUrl: './app.component.html',
  styleUrl: './app.component.css'
})
export class AppComponent {
  constructor() {
    Amplify.configure(outputs)
  }
}
```
**Bước 2:** chạy command sau: 
```
npm add @aws-amplify/ui-angular
```
**Bước 3:** import AmplifyAuthenticatorModule vào login component.
```typescript
import { Component } from '@angular/core'; 
import { AmplifyAuthenticatorModule } from '@aws-amplify/ui-angular';

@Component({
  selector: 'app-login',
  standalone: true,
  imports: [AmplifyAuthenticatorModule],
  templateUrl: './login.component.html',
  styleUrl: './login.component.css'
})
export class LoginComponent {

}
```
**Bước 4:** copy đoạn code sau bỏ vào template của login component.
```html \\ login.component.html
<amplify-authenticator>
  <ng-template
    amplifySlot="authenticated"
    let-user="user"
    let-signOut="signOut"
  >
    <h1>Welcome {{ user.username }}!</h1>
    <button (click)="signOut()">Sign Out</button>
  </ng-template>
</amplify-authenticator>
```
**Bước 5: thêm dòng sau vào mục architect.builder của angular.json: 
```
"styles": [
    "node_modules/@aws-amplify/ui-angular/theme.css",
    "src/styles.scss"
],

```
**Bước 6:** thêm hub event để bắt sự kiện signIn và navigate qua route 'home'
```typescript
import { Component } from '@angular/core';
import { AmplifyAuthenticatorModule } from '@aws-amplify/ui-angular';
import { Hub } from 'aws-amplify/utils';
import { Router } from '@angular/router';
@Component({
  selector: 'app-login',
  standalone: true,
  imports: [AmplifyAuthenticatorModule],
  templateUrl: './login.component.html',
  styleUrl: './login.component.css'
})
export class LoginComponent {
  constructor(private router: Router) {
    Hub.listen('auth', (data) => {
      this.onAuthEvent(data);
    });
  }

  onAuthEvent(payload: any) {
    if (payload.event === "signedIn")
      this.router.navigate([''])
  }
}

```
Sau phần này, trang login của chúng ta được như sau: 

![login](/images/4.buildApp/4.2-frontend/pic2.png)


