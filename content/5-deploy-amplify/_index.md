+++
title = 'Deploy App lên Amplify'
date = 2024-08-30T16:50:30+07:00
draft = false
pre = '5. '
weight = 5
+++
Ở phần này, chúng ta sẽ tìm hiểu cách Deploy 1 trang web lên amplify nhé.
### Chuẩn bị
- aws account
- github account
- git CLI

{{% notice info %}}
Nếu bạn chưa có tài khoản github, bạn có thể tạo tài khoản ở [đây](https://github.com/) và xem hướng dẫn tạo tài khoản ở [đây](https://docs.github.com/en/get-started/start-your-journey/creating-an-account-on-github)
{{% /notice %}}
**Bước 1:** Sau khi đăng nhập github, nhấp New
![pic1](/images/5.deploy/pic1.png)
- Nhập tên repo là: Trello-fullstack
- nhấp create repository.
![pic2](/images/5.deploy/pic2.png)
**Bước 2:** Về lại project dưới local, để đẩy từ local lên global, ta chạy các command sau: 
```
git add .
git commit -m "first commit"
git remote add origin https://github.com/<your-username>/Trello-fullstack.git
git push -u orign master
```
**Bước 3:** Đăng nhập vào aws(iam cấp quyền hay root đều được)
- Trên thanh tìm kiếm, search amplify.
- Chọn Aws Amplify.
![pic4](/images/5.deploy/pic4.png)
**Bước 4:** Khi màn hình chính của amplify hiện ra: 
- click create new app
![pic5](/images/5.deploy/pic5.png)
**Bước 6:** 
- Chọn Github.
- Click Next.
![pic6](/images/5.deploy/pic6.png)
**Bước 7:** 
- click "Update Github permission"
![pic7](/images/5.deploy/pic7.png)
**Bước 8:**: 1 cửa sổ github hiện lên.
- Click "Select repository"
- Nhập tên Repo vừa tạo ở Bước 1 vào.
- Click "Save".
![pic8](/images/5.deploy/pic9.png)
**Bước 9:** Quay lại cửa sổ Amplify
- Search trên thanh tìm kiếm của Amplify đã xuất hiện tên của repo vừa thêm vào.
- Sau khi chọn project, click Next.
![pic9](/images/5.deploy/pic10.png)
**Bước 10:**
- Tại phân Build Output Directory, chỉnh sửa thành dist/fullstack-crud/browser. Vì đây mới là thư mục build của angular.
- Click Next
- Click "Save and Deploy".
![pic10](/images/5.deploy/pic11.png)
Tới đây, đợi amplify build và deploy cho chúng ta là được.
{{% notice warn %}}
Chúng ta cần sửa phần architect.build.configurations.productions.budget[0].maximuError tăng lên 2mb. Vì app chúng ta build lên nặng tầm khoảng 1.38mb, là vượt ngưỡng maximumError ban đầu là 1mb. Nếu không sửa thì sẽ nhảy ra bug, amplify sẽ không build thành công. 
{{% /notice %}}
![pic11](/images/5.deploy/pic12.png)
Sau khi màn hình đã hiển thị "Deployed" là chúng ta đã deploy trang web thành công.