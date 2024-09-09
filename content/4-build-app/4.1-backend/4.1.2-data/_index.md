+++
title = 'Data'
date = 2024-08-30T16:49:49+07:00
draft = false 
weight = 2
pre = "<b>2. </b>"
+++
**Bước 1:** mở file amplify/data/resource.ts  
![data path](/images/4.buildApp/4.1-Backend/pic2.png)
**Bước 2:** update biến schema như sau: 
```typescript
const schema = a.schema({
  Column: a
    .model({
      owner: a.string().authorization(allow => [allow.owner().to(['read', 'delete'])]),
      title: a.string().required(),
      order: a.integer().required(),
      cards: a.hasMany("Card", "columnId"),
      cardsList: a.json().array()
    })
    .authorization((allow) => [allow.ownerDefinedIn("owner")])
    .secondaryIndexes(index => [
      index("owner")
      .queryField("listColumnsByOrder")
      .sortKeys(["order"])
    ]),
  Card: a
    .model({
      title: a.string().required(),
      order: a.integer(),
      columnId: a.id(),
      column: a.belongsTo("Column", "columnId")
    })
    .authorization((allow) => [allow.owner()])
    .secondaryIndexes(index => [
      index("columnId")
      .queryField("listCardsByOrder")
      .sortKeys(["order"])
    ]),
});
```
Đoạn code trên có tác dụng định nghĩa 2 data model là column và card. Column và card có 1 mối quan hệ 1 nhiều,
1 columns có thể có nhiều card, nhưng 1 card thì chỉ có duy nhất 1 column trong cùng 1 thời điểm.
{{% notice note %}}
Chúng ta không cần phải set những trường như id, createdAt, updatedAt vì amplify đã tự động làm điều đó cho chúng ta. 
{{% /notice %}}
{{% notice note %}}
Đoạn code trên cũng set cơ chế authorization là owner(tức là chỉ có owner mới có quyền truy cập vào tài nguyên này). Và cơ chế authorization là owner thì amplify sẽ tự đông thêm 1 trường owner vào data để xác định data là của user nào.
{{% /notice %}}
{{% notice warning %}}
Vì authorization mà chúng ta đang sử dụng là owner() nên cần phải config authMode là "userPool". 
{{% /notice %}}
Tiếp theo, chúng ta sẽ code phần Frontend.
