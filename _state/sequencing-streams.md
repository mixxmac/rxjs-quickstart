---
title: Sequencing Streams 
position: 2.3
wistia_id: h83z5yly2k
description: Sequencing streams for complex operations
right_code: |
  ~~~ typescript
  import { Component, OnInit, ViewChild } from '@angular/core';
  import { Observable } from 'rxjs/Observable';
  import 'rxjs/add/observable/fromEvent';
  import 'rxjs/add/operator/map';
  import 'rxjs/add/operator/startWith';
  import 'rxjs/add/operator/switchMap';
  import 'rxjs/add/operator/takeUntil';
  
  @Component({
    selector: 'app-triggers',
    template: `
    <div #ball class="ball"
      [style.left]="position.x + 'px'"
      [style.top]="position.y + 'px'">
    </div>
    `
  })
  export class TriggersComponent implements OnInit {
    @ViewChild('ball') ball;
    position: any;
  
    ngOnInit() {
      const BALL_OFFSET = 50;
  
      const move$ = Observable.fromEvent(document, 'mousemove')
        .map((event: MouseEvent) => {
          const offset = $(event.target).offset();
          return {
            x: event.clientX - offset.left - BALL_OFFSET,
            y: event.pageY - BALL_OFFSET
          };
        });
  
      const down$ = Observable.fromEvent(this.ball.nativeElement, 'mousedown')
        .do(event => this.ball.nativeElement.style.pointerEvents = 'none');
  
      const up$ = Observable.fromEvent(document, 'mouseup')
        .do(event => this.ball.nativeElement.style.pointerEvents = 'all');
  
      down$
        .switchMap(event => move$.takeUntil(up$))
        .startWith({ x: 100, y: 100})
        .subscribe(position => this.position = position);
    }
  }
  ~~~
---

Bacon ipsum dolor amet chuck short ribs t-bone tenderloin. Meatloaf rump alcatra swine filet mignon corned beef tongue leberkas tail salami shoulder venison strip steak shankle hamburger. Pork loin leberkas brisket, frankfurter pig corned beef tongue beef ribs swine jerky tenderloin. Andouille brisket swine, jowl cow jerky kevin sausage.
