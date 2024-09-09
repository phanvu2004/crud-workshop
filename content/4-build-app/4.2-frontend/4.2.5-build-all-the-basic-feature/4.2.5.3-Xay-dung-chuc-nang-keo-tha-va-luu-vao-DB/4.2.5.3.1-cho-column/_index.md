+++
title = 'Xây dựng chức năng kéo thả cho Column'
date = 2024-09-09T14:46:48+07:00
draft = false
weight = 1
pre = '4.2.5.3.1 '
+++
![column](/images/4.buildApp/4.2-frontend/pic13.png)
Chúng ta sẽ build chức năng kéo thả cho column dựa trên sequence diagram sau:

**Bước 1:** Tạo hàm updateInstance trong crudService như sau: 
{{< tabs groupId="dndCrud" >}}
    {{% tab name="src/app/home/crud.service.ts" %}}
``` typescript 
export class CrudService {
 /* ... */ 
  async updateInstance(
    instance: 'Column' | 'Card',
    data: {id: string ,title?: string, order?: number, columnId?: string }
  ) {
    const result = await client.models[instance].update(data);
    return result;
  }
  /* ... */
}
```
    {{% /tab %}}
{{< /tabs>}}

{{% notice note %}}
hàm updateInstace này sẽ dùng chung cho cả column và card. Nên ở phần sau, chỉ lấy ra xài chứ kh tạo lại hàm này.
{{% /notice %}}
**Bước 2:** Update homeComponent như sau để import cdkDrag từ angular material cho việc triển khai chức năng kéo thả Column:
{{< tabs groupId="dndHome" >}}
    {{% tab name="src/app/home/home.component.ts" %}}
``` typescript 
/* ... */
import { CdkDragDrop,
  moveItemInArray,
  CdkDrag,
  CdkDropList,
} from '@angular/cdk/drag-drop';
@Component({
  /* ... */
  imports: [
    ToolbarComponent,
    ColumnComponent,
    AddColumnComponent,
    CdkDrag,
    CdkDropList
  ],
  /* ... */
})
export class HomeComponent implements OnInit {
    /* ... */
  drop(event: CdkDragDrop<any[]>) {
    moveItemInArray(this.columns!, event.previousIndex, event.currentIndex)
    this.columns?.forEach(async (elem, index) => {
      if (elem.order === index)
        return;

      await this.crud.updateInstance('Column', {
        id: elem.id,
        order: index
      })
      elem.order = index;
    })
  }
/* ... */
}
```
    {{% /tab %}}
    {{% tab name="src/app/home/home.component.html" %}}
```html
<app-toolbar></app-toolbar>

<div class="columns" cdkDropListOrientation="horizontal"  cdkDropList (cdkDropListDropped)="drop($event)">
  @for(column of columns; track column) {
    <app-column class="column" [columns]="columns" [column]="column" cdkDrag></app-column>
  }
  <app-add-column (emitTitle)="createColumn($event)"></app-add-column>
</div>
```
    {{% /tab %}}
    {{% tab name="src/app/home/home.component.css" %}}
```css
/* ... */
.cdk-drag-placeholder {
  opacity: 0;
}

.cdk-drag-animating {
  transition: transform 250ms cubic-bezier(0, 0, 0.2, 1);
}

.columns.cdk-drop-list-dragging .column:not(.cdk-drag-placeholder) {
  transition: transform 250ms cubic-bezier(0, 0, 0.2, 1);
}
```
    {{% /tab %}}
{{< /tabs>}}