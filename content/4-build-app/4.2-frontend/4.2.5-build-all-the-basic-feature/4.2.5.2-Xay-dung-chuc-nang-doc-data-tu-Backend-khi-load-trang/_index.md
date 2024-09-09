+++
title = ' Xây Dựng Chức Năng Đọc Data Từ Backend khi load trang'
date = 2024-09-09T10:12:59+07:00
draft = false
pre = "4.2.5.2 "
weight = 2
+++
**Bước 1:** triển khai hàm readAll ở crud service:
{{< tabs groupId="readAllCrud" >}}
    {{% tab name="src/app/home/crud.service.ts" %}}
```typescript
import { AuthService } from '../auth.service';
/* ... */

@Injectable({
  providedIn: 'root'
})
export class CrudService {
  constructor(private auth: AuthService) {}
  async readAll() {
    const user = await this.auth.getUser()
    const { data } = await client.models.Column.listColumnsByOrder({
      owner: `${user.userId}::${user.username}`,
      order: {
        ge: 0
      }
    }, { sortDirection: "ASC" })

    data.forEach(async column => {
      const { data } = await client.models.Card.listCardsByOrder({
        columnId: column.id,
        order: {
          ge: 0
        }
      }, { sortDirection: "ASC"})

      column.cardsList = data;
    })

    return data;
  }
}
```
    {{% /tab %}}  
{{< /tabs >}}

{{% notice info %}}
Nguyên lí của hàm readAll là: query bằng secondaryIndex mà chúng ta đã tạo cho cả 2 model Column và Card trong phần 4.1.2 và sắp xếp các records theo trường "order" theo thứ tự từ bé tới lớn.
{{% /notice %}}

**Bước 2:** Sử dụng hàm readAll trong component home để mỗi lần tải lại trang, trang web sẽ load toàn bộ column và card trong database thuộc về user đó và render ra cho người dùng: 

{{< tabs groupId="getAllHome" >}}
    {{% tab name="src/app/home/home.component.ts" %}}
``` typescript
import { Component, OnInit } from '@angular/core';
/* ... */

export class HomeComponent implements OnInit { 
    /* ... */
    ngOnInit(): void {
        (async () => {
            this.columns = await this.crud.readAll()
        })()
    }
    /* ... */
}
```
    {{% /tab %}}  
{{< /tabs >}}

Bây giờ, sau khi đăng nhập, bạn thử tạo 1 vài column và card và reload trang, ta sẽ thấy những column và card đó vẫn giữ nguyên. Vậy là ta đã hoàn thành việc implement chức năng đọc data từ DB.
Tiếp theo, chúng ta sẽ xây dựng phần kéo thả column và card.
