+++
title = 'Authentication'
date = 2024-08-30T16:49:38+07:00
draft = false 
weight = 1
pre = "<b>1. </b>"
+++
**Bước 1:** từ root của project, mở file ./amplify/auth/resource.ts
![auth](/images/4.buildApp/4.1-Backend/pic1.png)
**Bước 2:** update auth bằng đoạn code sau: 
```typescript
export const auth = defineAuth({
  loginWith: {
    email: true
  },
  userAttributes: {
    preferredUsername: {
      mutable: true,
      required: true
    }
  }
});

```
Tiếp theo, chúng ta sẽ config phần data. 

