+++
title = 'Khởi tạo project'
date = 2024-08-30T16:16:43+07:00
weight = 3
pre = "<b>3. </b>"
+++

Trong phần này, chúng ta sẽ cùng nhau tìm hiểu cách tạo FE angular project và cách thêm amplify gen 2 vào project nhé
## Boilerplate
Nếu bạn là người mới với angular và amplify thì mình recommend bạn nên sử dụng boilerplate mẫu mà mình đã tạo sẵn bằng cách chạy command sau ở terminal.  
```
git clone https://github.com/phanvu2004/amplify-boilerplate.git

```

## Manual setup
### Triển khai angular project
**Bước 1:** chạy command dưới đây.  
```
ng new fullstack-crud
```
**Bước 2:** chọn stylesheet format cho web app của bạn. Ở đây mình chọn css.
![pick stylesheet](/images/3.setupProject/pic1.png) 
**Bước 3:** Cho phép web app của bạn chạy ssr hay không. Ở đây mình vẫn chọn **Có**.
![pick ssr or not](/images/3.setupProject/pic2.png) 


{{% notice info %}}
SSR là 1 cơ chế mà server sẽ tạo ra 1 khung trang web rồi gửi xuống trình duyệt. Ngược với csr, trình duyệt sẽ đảm nhận công việc này.  
Xem thêm tại: https://angular.dev/guide/ssr
{{% /notice %}}
**Bước 4:** Chạy project.
Sau khi angular khởi tạo xong 1 angular project, Bạn có thể cd vào project:  
```
cd fullstack-crud
```
và chạy project bằng 
```
ng serve 
```
![browser](/images/3.setupProject/pic3.png)
Vậy là bạn đã tạo xong 1 angular project.
### Thêm amplify gen 2 vào angular project
Chạy command sau: 
```
npm create amplify@latest
```
Sau khi chạy xong, trong project sẽ có thêm 1 thư mục amplify. Vậy là chúng ta đã thêm amplify gen 2 vào FE project thành công. Tiếp theo, chúng ta sẽ cùng build app nhé.
