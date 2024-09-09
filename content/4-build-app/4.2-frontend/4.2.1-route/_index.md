+++
title = ' Chia Route'
date = 2024-08-30T16:46:54+07:00
draft = false 
weight = 1
pre = "4.2.1"
+++
**Bước 1:** chạy 2 câu lệnh sau để tạo 3 component home, login và pageNotFound cho trang web.
```
ng g c login 
ng g c home
ng g c pageNotFound
```
{{% notice info %}}
  Mỗi component sẽ ứng với 1 route là : /login, /home và pageNotFound ứng với các route không tồn tại.  
{{% /notice  %}}
**Bước 2:** mở file /src/app/app.route.ts
    + update route như sau:
  {{< tabs groupId="appRoute">}}
    {{% tab name="src/app/app.route.ts" %}}
``` typescript
export const routes: Routes = [
  {
    path: '',
    component: HomeComponent
  },
  {
    path: 'home',
    redirectTo: ''
  },
  {
    path: 'login',
    component: LoginComponent
  },
  {
    path: "**",
    component: PageNotFoundComponent
  }
];
```
    {{% /tab %}}
 {{< /tabs >}}  
TIếp theo, chúng ta sẽ đi xây dựng trang login cho trang web.