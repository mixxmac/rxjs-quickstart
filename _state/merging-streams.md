---
title: Merging Streams
position: 2.1
description: Merging two streams into a single stream
wistia_id: 25k1htpl5h

right_code: |
  ~~~ typescript
  import { Component, OnInit, ViewChild } from '@angular/core';
  import { Observable } from 'rxjs/Observable';
  import 'rxjs/add/observable/fromEvent';
  import 'rxjs/add/observable/merge';
  import 'rxjs/add/operator/map';
  import 'rxjs/add/operator/scan';
  import 'rxjs/add/operator/startWith';
  
  @Component({
    selector: 'app-merging-streams',
    template: `
    <button #left md-raised-button color="accent">Move Left</button>
    <button #right md-raised-button color="accent">Move Right</button>
    <div class="container">
      <div #ball class="ball"
        [style.left]="position.x + 'px'"
        [style.top]="position.y + 'px'">
      </div>
    </div>
    `
  })
  export class MergingStreamsComponent implements OnInit {
    @ViewChild('left') left;
    @ViewChild('right') right;
    position: any;
  
    ngOnInit() {
      const left$ = Observable.fromEvent(this.getNativeElement(this.left), 'click')
        .map(event => -10);
  
      const right$ = Observable.fromEvent(this.getNativeElement(this.right), 'click')
        .map(event => 10);
  
      Observable.merge(left$, right$)
        .startWith({x: 200, y: 200})
        .scan((acc, curr) => Object.assign({}, acc, {x: acc.x + curr}))
        .subscribe(position => this.position = position);
    }
  
    getNativeElement(element) {
      return element._elementRef.nativeElement;
    }
  }
  ~~~
---