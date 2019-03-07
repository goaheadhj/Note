



```typescript 
import { Component,OnInit,Input } from '@angular/core'; 

import { data } from '../data'; 

import {ITreeNode} from '../../model/tree-node'; 

import {Tree} from '../../type/tree'; 

@Component({ 

selector: 'tree-view', 

template: ` 

<ul class="tree"> 

  <ng-template #recursiveList let-list="list"> 

  <li *ngFor="let item of list"> 

  {{item.name}} 

  <ul *ngIf="item.childs?.length > 0"> 

  <ng-container *ngTemplateOutlet="recursiveList; context:{ list: item.childs}"></ng-container> 

  </ul> 

  </li> 

  </ng-template> 

  <ng-container *ngTemplateOutlet="recursiveList; context:{ list: data.childs}"></ng-container> 

</ul> 

`, 

styleUrls: ['./tree-view.component.css'] 

}) 

export class TreeViewComponent implements OnInit { 

@Input() node:ITreeNode; 

data:any; 

ngOnInit(){ 

this.data=this.node; 

console.log(this.node); 

} 

} 
```