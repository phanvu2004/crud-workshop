+++
title = 'Xây Dựng Chức Năng Tạo Column Và Card'
date = 2024-09-08T22:39:46+07:00
draft = false
weight = 1
pre = '4.2.5.1 '
+++
Logic tạo Column được triển khai theo sơ đồ sau: 
![sequence diagram tạo column](/images/4.buildApp/4.2-frontend/pic6.png)  
Và logic tạo Card được triển khai theo sơ đồ sau:
![sequence diagram tạo card](/images/4.buildApp/4.2-frontend/pic7.png)  

Như ta có thể thấy, logic của 2 chức năng này là tương tự nhau, nên chúng ta sẽ triển khai chúng cùng 1 lúc.

**Bước 1:** Chạy 2 command sau để tạo các component sau: add-column, column, card và add-card.
{{< tabs groupId="createAdd" >}}
    {{% tab name="./src" %}}
```
ng g c home/add-column
ng g c home/column
ng g c home/column/card
ng g c home/column/add-column
ng g s home/crud
```
    {{% /tab %}}
{{< /tabs >}}
**Bước 2:** import Data client và triển khai các hàm tạo Column và tạo Card trong crud service.
{{< tabs groupId="crud" >}}
    {{% tab name="./src/app/home/crud.service.ts" %}}
```typescript
import { Injectable } from '@angular/core';
import { generateClient } from 'aws-amplify/data';
import { Schema } from '../../../amplify/data/resource';
import { AuthService } from '../auth.service';

const client = generateClient<Schema>()
@Injectable({
  providedIn: 'root'
})
export class CrudService {

  constructor(private auth: AuthService) { }
  async createInstance(
    instance: 'Column' | 'Card',
    data: { title: string, order: number, columnId?: string, cardsList?: Array<any> }
  ) {
    const result = await client.models[instance].create(data);
    return result;
  }

   async createColumn(
    data: { title: string, order: number }
  ) {
    const result = await this.createInstance("Column", {
        ...data,
        cardsList: []
    })
    return result;
  }

  async createCard(
    data: { title: string, order: number, columnId: string }
  ) {
    const result = await this.createInstance("Card", data)
    return result;
  }
}
```
    {{% /tab %}}
{{< /tabs >}}

**Bước 3:** Xây dựng design và chức năng emit Title cho 2 component add-column và add-card:
- add-column: 
{{< tabs groupId="addColumn" >}}
    {{% tab name="./src/app/home/add-column.component.ts" %}}
``` typescript
import { Component, Output, EventEmitter } from '@angular/core';
import {MatInputModule} from '@angular/material/input';
import {MatFormFieldModule} from '@angular/material/form-field';
import { MatButtonModule } from '@angular/material/button';
@Component({
  selector: 'app-add-column',
  standalone: true,
  imports: [
    MatInputModule,
    MatFormFieldModule,
    MatButtonModule
  ],
  templateUrl: './add-column.component.html',
  styleUrl: './add-column.component.css'
})
export class AddColumnComponent {
  @Output()
  emitTitle = new EventEmitter<string>();

  editState = false;
  changeState() {
    this.editState = !this.editState
  }

  submit(title: string) {
    this.changeState()
    if (title.length)
      this.emitTitle.emit(title)
  }
}

```
    {{% /tabs %}}
    {{% tab name="./src/app/home/add-column.component.html" %}}
``` html
@if (editState) {
<mat-form-field class="example-full-width">
  <input matInput placeholder="column title" #input>
  <button mat-button type="button" (click)="submit(input.value)">submit</button>
  <button mat-button type="button" (click)="changeState()">Cancel</button>
</mat-form-field>
}
@else {
  <button mat-button type="button" (click)="changeState()">add a button</button>
}

```
    {{% /tabs %}}
{{< /tabs >}}

- add-card: 
{{< tabs groupId="addCard" >}}
    {{% tab name="./src/app/home/column/add-card.component.ts" %}}
``` typescript
import { Component, Output, EventEmitter } from '@angular/core';
import {MatInputModule} from '@angular/material/input';
import { MatButtonModule } from '@angular/material/button';

@Component({
  selector: 'app-add-card',
  standalone: true,
  imports: [
    MatInputModule,
    MatButtonModule
  ],
  templateUrl: './add-card.component.html',
  styleUrl: './add-card.component.css'
})
export class AddCardComponent {
  editState = false;
  @Output()
  emitTitle = new EventEmitter<string>();
  changeState() {
    this.editState = !this.editState
  }

  submit(title: string) {
    this.changeState()
    if (title.length)
      this.emitTitle.emit(title)
  }
}

```
    {{% /tabs %}}
    {{% tab name="./src/app/home/column/add-card.component.html" %}}
``` html
@if (editState) {
<mat-form-field class="example-full-width">
  <input matInput placeholder="column title" #input>
  <button mat-button type="button" (click)="submit(input.value)">submit</button>
  <button mat-button type="button" (click)="changeState()">Cancel</button>
</mat-form-field>
}
@else {
  <button mat-button type="button" (click)="changeState()">add a button</button>
}

```
    {{% /tabs %}}
{{< /tabs >}}

**Bước 4:** Tiếp theo, triển khai hàm createColumn trong home Component như sau:  
{{< tabs groupId="homeAddColumn" >}}
    {{% tab name="./src/app/home/home.component.ts" %}}
``` typescript
import { Component } from '@angular/core';
import { ColumnComponent } from './column/column.component';
import { CrudService } from './crud.service';
import { ToolbarComponent } from './toolbar/toolbar.component';
import { AddColumnComponent } from './add-column/add-column.component';


@Component({
  selector: 'app-home',
  standalone: true,
  imports: [
    ToolbarComponent,
    ColumnComponent,
    AddColumnComponent
  ],
  templateUrl: './home.component.html',
  styleUrl: './home.component.css'
})

export class HomeComponent {
  constructor(private crud: CrudService) {}
  columns?: Array<any>;

  async createColumn(title: string) {
    const { data } = await this.crud.createColumn({
      title: title,
      order: this.columns?.length ?? 0
    })
    this.columns = [
      ...this.columns,
      data
    ]
  }
}

```
    {{% /tab %}}
    {{% tab name="./src/app/home/home.component.html" %}}
``` html
<app-toolbar></app-toolbar>

<div class="columns">
  @for(column of columns; track column) {
    <app-column class="column" [column]="column" cdkDrag></app-column>
  }
  <app-add-column (emitTitle)="createColumn($event)"></app-add-column>
</div>
```
    {{% /tab %}}
    {{% tab name="./src/app/home/home.component.css" %}}
``` css
.columns {
  padding-left: 10px;
  display: flex;
  flex-direction: row;
  gap: 10px;
  max-width: 100%;
  overflow-x: auto;
}
```
    {{% /tab %}}
{{< /tabs >}}

**Bước 5:** Tạo design và xây dựng chức năng tạo card cho component column:
{{< tabs groupId="createColumn" >}}
    {{% tab name="./src/app/home/column/column.component.ts" %}}
```typescript
import { Component, Input, OnChanges } from '@angular/core';
import { CardComponent } from './card/card.component';
import { AddCardComponent } from './add-card/add-card.component';
import { CrudService } from '../crud.service';
import {MatIconModule} from '@angular/material/icon';
import {MatButtonModule} from '@angular/material/button';

@Component({
  selector: 'app-column', 
  standalone: true, 
  imports: [
    CardComponent,
    AddCardComponent,
    MatButtonModule,
    MatIconModule
  ],
  templateUrl: './column.component.html',
  styleUrl: './column.component.css'
})
export class ColumnComponent implements OnChanges {

  constructor(private crud: CrudService) {}
  @Input()
  column: any

  @Input()
  columns?: any[]
  columnIds?: string[]

  async addCard(title: string) {
    const { data } = await this.crud.createCard({
      columnId: this.column.id,
      title: title,
      order: this.column?.cardsList?.length || 0
    })

    if (!this.column?.cardsList)
      this.column.cardsList = [ data ];
    else 
      this.column.cardsList.push(data);
  }
}
```
    {{% /tab %}}
    {{% tab name="./src/app/home/column/column.component.html" %}}
```html
<div class="column">
  <header>
    <h3>{{column.title}}</h3>
    <button type="button" mat-icon-button>
      <mat-icon>close</mat-icon>
    </button>
  </header>
  <div class="cards"
    [id]="column.id">
    @for (card of column.cardsList; track card) {
    <app-card [card]="card"></app-card>
    }
  </div>
  <app-add-card (emitTitle)="addCard($event)"></app-add-card>
</div>
```
    {{% /tab %}}
    {{% tab name="./src/app/home/column/column.component.css" %}}
```css
.column {
  width: 350px;
  padding-left: 10px;
  padding-right: 10px;
  background-color: #c0c0c0;
  border-radius: 5px;
  padding-bottom: 10px;
}

.cards {
  display: flex;
  flex-direction: column;
  min-height: 5px;
  gap: .5em;
}

header {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
}

h3 {
  margin-top: 15px;
}
```
    {{% /tab %}}
{{< /tabs >}}
**Bước 6:** Tạo giao diện cho component Card:
{{< tabs groupId="createCard" >}}
    {{% tab name="./src/app/home/column/card/card.component.ts" %}}
``` typescript
import { Component, Input } from '@angular/core';
import {MatIconModule} from '@angular/material/icon';
import {MatButtonModule} from '@angular/material/button';
import {MatCardModule} from '@angular/material/card';
@Component({
  selector: 'app-card',
  standalone: true,
  imports: [
    MatCardModule,
    MatButtonModule,
    MatIconModule
  ],
  templateUrl: './card.component.html',
  styleUrl: './card.component.css'
})
export class CardComponent {
  @Input()
  card: any;
}
```
    {{% /tab %}}
    {{% tab name="./src/app/home/column/card/card.component.html" %}}
```html
<mat-card appearance="outlined" color="accent">
  <mat-card-content>{{card.title}}</mat-card-content>
  <button type="button" mat-icon-button>
    <mat-icon>close</mat-icon>
  </button>
</mat-card>
```
    {{% /tab %}}
{{< /tabs >}}

- Sau đó giao diện trang web sẽ trông như sau: 
![create interface](/images/4.buildApp/4.2-frontend/pic8.png)
- Sau khi bấm vào button được khoanh đỏ, sẽ ra 1 hộp thoại như sau: 
![dialog create column](/images/4.buildApp/4.2-frontend/pic9.png)
- Nhập title bạn muốn r click vào nút submit được khoanh đỏ:
![create a column](/images/4.buildApp/4.2-frontend/pic10.png)
- Ta đã tạo thành công tạo 1 column và làm tương tự, ta sẽ tạo được 1 card.
![create a card](/images/4.buildApp/4.2-frontend/pic11.png)
Tiếp theo, Chúng ta sẽ đi đến phần Đọc data từ Backend lúc load trang.