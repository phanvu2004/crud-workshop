+++
title = 'Xây dựng chức năng kéo thả Cho Card'
date = 2024-09-09T14:46:55+07:00
draft = false
weight = 2
pre = '4.2.5.3.2 '
+++
![column](/images/4.buildApp/4.2-frontend/pic12.png)
Để xây dựng chức năng kéo thả dựa trên sequence diagram trên, ta update column component như sau: 
{{< tabs groupId="dndColumn" >}}
    {{% tab name="src/app/home/column/column.component.ts" %}}
```typescript
import {
  CdkDragDrop,
  moveItemInArray,
  transferArrayItem,
  CdkDrag,
  CdkDropList,
} from '@angular/cdk/drag-drop';

@Component({
   /* ... */
  imports: [
    CardComponent,
    AddCardComponent,
    CdkDrag,
    CdkDropList,
    MatButtonModule,
    MatIconModule
  ],
   /* ... */
})
export class ColumnComponent implements OnChanges {
  @Input()
  columns?: any[]
  columnIds?: string[]

  drop(event: CdkDragDrop<any[]>) {
    if (event.previousContainer === event.container) {
      moveItemInArray(event.container.data, event.previousIndex, event.currentIndex);
    } else {
      transferArrayItem(
        event.previousContainer.data,
        event.container.data,
        event.previousIndex,
        event.currentIndex,
      );
    }

     event.container.data.forEach(async (card, index ) => {
      if (index === card.order && card.columnId === this.column.id)
        return;

      await this.crud.updateInstance("Card", {
        id: card.id,
        order: index,
        columnId: this.column.id
      })

      card.order = index
    })

    ngOnChanges(): void {
        this.columnIds = this.columns?.map(elem => elem.id).filter(id => id !== this.column.id)
    }
  }
}
```
    {{% /tab %}}
    {{% tab name="src/app/home/column/column.component.html" %}}
``` html
<!--... -->
  <div class="cards" cdkDropList
    [id]="column.id"
    (cdkDropListDropped)="drop($event)"
    [cdkDropListData]="column.cardsList"
    [cdkDropListConnectedTo]="columnIds!">
    @for (card of column.cardsList; track card) {
    <app-card [card]="card" cdkDrag></app-card>
    }
  </div>
  <app-add-card (emitTitle)="addCard($event)"></app-add-card>
</div>

```
    {{% /tab %}}
{{< /tabs>}}

{{% notice info %}}
Không thể sử dụng cdkDropListGroup mà cdkDragAndDrop cung cấp, vì vậy, phải sử dụng logic riêng để kết nối các column lại với nhau. Bằng cách truyền thêm Columns từ home component xuống và map ra id của columns rồi filter những id không bằng với id của column đó và bỏ vào attribute cdkDropListConnectedTo
{{% /notice %}}

Đến đây, trang web Trello của chúng ta sẽ trông như sau: 
#video

