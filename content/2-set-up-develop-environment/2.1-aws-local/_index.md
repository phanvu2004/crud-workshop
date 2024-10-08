+++
title = "Cài đặt AWS cho môi trường local"
draft = false
date = 2024-08-30T16:38:38+07:00
weight = 1
pre = "<b>2.1 </b>"
+++
### Chuẩn bị
 + Terminal (để chạy các câu lệnh AWS CLI)
 + Tài khoản AWS.

{{% notice info %}}
Nếu bạn chưa có tài khoản AWS, có thể tham khảo cách tạo tài khoản tại [đây](https://000001.awsstudygroup.com/1-create-new-Aws-account/)
{{% /notice %}}

### Kích hoạt IAM Identity Center và AWS Organization.
**Bước 1:** ĐăNG nhập vào tài khoản root AWS hoặc IAM có quyền admin.
+ Tìm kiếm IAM Identity Center trên thanh tìm kiếm
+ Click vào IAM Identity Center.

![Search IAM Identity Center](/images/2.set-up-dev-local/2.1-aws-local/pic2.png)
**Bước 2:** click **Enable**

![enable Iam](/images/2.set-up-dev-local/2.1-aws-local/pic1.png)

**Bước 2:** click **Continue**
![enable Iam](/images/2.set-up-dev-local/2.1-aws-local/pic3.png)
**Bước 3:** Tạo Permission set.
+ Sau khi được navigate vào dashboard của IAM Identity Center, click permission.

![click permission](/images/2.set-up-dev-local/2.1-aws-local/pic4.png)
+ click **Create permission set**
+ Chọn custom permission set.
+ Tại mục AWS managed Policies, chọn quyền **AmplifyBackendDeployFullAccess**.
![click permission](/images/2.set-up-dev-local/2.1-aws-local/pic5.png)
+ Sau đó, đặt tên cho permission set và click **Next**.
![click permission](/images/2.set-up-dev-local/2.1-aws-local/pic6.png)
+ Sau khi kiểm tra lại thông tin thì click **Create**. Bạn sẽ được điều hướng về dashboard.  
**Bước 4 :**
+ Chọn User -> Create User.
+ Đặt tên cho user, kiểm tra lại thông tin và click **Create**.
![click permission](/images/2.set-up-dev-local/2.1-aws-local/pic7.png)
+ Navigate qua AWS account.
+ Chọn account mà bạn dùng để tạo organization. Click **Asign users or groups**.
![click permission](/images/2.set-up-dev-local/2.1-aws-local/pic8.png)
+ Chọn user mà mình vừa tạo.
![click permission](/images/2.set-up-dev-local/2.1-aws-local/pic10.png)
+ Gán permission set mà bạn đã tạo trước đó với user này. 
+ Kiểm tra lại thông tin và submit. 
+ Sau khi tạo xong tài khoản, tại trang dashboard, có đường link dẫn tới AWS access portal URL.
+ Check gmail và làm theo hướng dẫn là bạn sẽ đăng nhập và đổi mật khẩu thành công. 
![click permission](/images/2.set-up-dev-local/2.1-aws-local/pic11.png)
## Tải AWS CLI 

{{< tabs >}}
{{% tab name="Linux" %}}
```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
./aws/install -i /usr/local/aws-cli -b /usr/local/bin
```
{{% /tab %}}
{{% tab name="Window" %}}
Tải và chạy aws cli msi installer cho window(64bit). 
[Tải ở đây.](https://awscli.amazonaws.com/AWSCLIV2.msi)
{{% /tab %}}
{{% tab name="Mac" %}}
Tải file macos pkg tại đây. 
[Tải ở đây.](https://awscli.amazonaws.com/AWSCLIV2.pkg)
{{% /tab %}}
{{< /tabs >}}

{{% notice warning %}}
Trong hệ điều hành linux, command `./aws/install -i /usr/local/aws-cli -b /usr/local/bin` cần được chạy dưới quyền sudo
{{% /notice %}}
## Cài đặt local aws profile
```
aws configure sso
```
Nhập thông tin
{{< tabs >}}
{{% tab name="Terminal" %}}
> SSO session name (Recommended): amplify-admin  
> SSO start URL: \<START SESSION URL>  
> SSO region: \<Region của bạn>   
> SSO registration scopes [sso:account:access]: \<để mặc định>  
> Attempting to automatically open the SSO authorization page in your default browser.  
> If the browser does not open or you wish to use a different device to authorize this request, open the following URL:  
> https://device.sso.us-east-2.amazonaws.com/  
> Then enter the code:  
> \<Code>
{{% /tab %}}  
{{< /tabs >}}
Nhập thông tin xong, trình duyệt sẽ tự động hỏi về authentication
{{< tabs >}}

{{% tab name="Terminal" %}}
The only AWS account available to you is: <your-aws-account-id>  
Using the the account ID <your-aws-account-id>  
The only role available to you is: amplify-policy  
Using the role name "amplify-policy"  
CLI default client Region [us-east-1]: <your-region>  
CLI default output format [None]:  
{{% /tab %}}
{{< /tabs >}}

{{% notice note %}}
Phải chắc chắn là tên profile được đặt tên là default
{{% /notice %}}

Sau đó, ~/.aws sẽ được tạo, bạn đã cài đặt thành công aws cho local.
Tiếp theo, chúng ta sẽ học cách cài đặt AngularV17 Cli
