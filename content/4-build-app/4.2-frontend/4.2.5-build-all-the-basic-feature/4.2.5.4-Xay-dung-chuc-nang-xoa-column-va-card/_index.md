+++
title = 'Xây Dựng Chức Năng Xóa Column Và Card'
date = 2024-09-09T15:52:52+07:00
draft = false
pre = '4.2.5.4 '
weight = 4
+++
Bây giờ, chúng ta sẽ thực hiện build chức năng cuối cùng, xóa column và xóa card.
**Bước 1:**  trong crud service, triển khai hàm deleteInstance như sau: 
{{< tabs groupId="deleteCruds" >}}
    {{% tab name="src/app/home/crud.service.ts" %}}
``` typescript
async deleteInstance(
    instance: 'Column' | 'Card',
    id: string
  ) {
    await client.models[instance].delete({id});
  }
```
    {{% /tab %}}
{{< /tabs>}}

**Bước 2:** Trong home component, tạo hàm deleteColumn: 
{{< tabs groupId="deleteHome" >}}
    {{% tab name="src/app/home/home.component.ts" %}}
``` typescript
  deleteColumn(id:string) {
    this.crud.deleteInstance("Column",  id)
    this.columns = this.columns?.filter(column => column.id != id)
  }
```
    {{% /tab %}}
{{< /tabs>}}
**Bước 3:** Trong column Component, triển khai như sau để hoàn chỉnh chức năng xóa Column
{{< tabs groupId="deleteColumn" >}}
    {{% tab name="src/app/home/column/column.component.ts" %}}
``` typescript
/*...*/
  constructor(private crud: CrudService, private home: HomeComponent) {}
  deleteColumn() {
    if (this.column.cardsList?.length)
      this.column.cardsList.forEach((card: any) => {
        this.deleteCard(card.id)
      });
    this.home.deleteColumn(this.column.id)
  }

  deleteCard(id: string) {
    this.crud.deleteInstance("Card", id)
    this.column.cardsList = this.column.cardsList.filter( (card: any) => card.id! !== id)
  }
/*...*/
```
    {{% /tab %}}
    {{% tab name="src/app/home/column/column.component.html" %}}
``` html
<!--... -->
  <header>
    <h3>{{column.title}}</h3>
    <button type="button" mat-icon-button (click)="deleteColumn()">
      <mat-icon>close</mat-icon>
    </button>
  </header>
<!--... -->
```
    {{% /tab %}}
{{< /tabs>}}

**Bước 4:** Xây dựng chức năng xóa card như sau: 
{{< tabs groupId="deleteCard" >}}
    {{% tab name="src/app/home/column/card/card.component.ts" %}}
```ts
  deleteCard() {
    this.column.deleteCard(this.card.id)
  }
```
    {{% /tab %}}
    {{% tab name="src/app/home/column/card/card.component.html" %}}
``` html
<mat-card appearance="outlined" color="accent">
  <mat-card-content>{{card.title}}</mat-card-content>
  <button type="button" mat-icon-button (click)="deleteCard()">
    <mat-icon>close</mat-icon>
  </button>
</mat-card>
```
    {{% /tab %}}
{{< /tabs>}}

Đến đây, bạn có thể xóa Column và Card bằng cách bấm vào nút "X" trên góc trên bên phải của column và card.