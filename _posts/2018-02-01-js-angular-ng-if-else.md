---
layout: post
title: '[Angular] ngIf에서 else 구문 사용하기'
date: 2018-02-01 00:00:00
categories: javascript
tags: [angular, til]
published: true
fullview: false
comments: true
---

## `if`(`templateName` 미사용)

```html
<div *ngIf="flag">
    flag 가 true 이면 보임
</div>
```

## `if`(`templateName` 미사용), `else`(`templateName` 사용)

```html
<div *ngIf="flag; else templateName">
    flag 가 true 이면 보임
</div>

<ng-template #templateName>
    flag 가 false 이면 보임
</ng-template>
```

## `if`(`templateName` 사용)

```html
<div *ngIf="flag; then templateName">
    절대 보이지 않음
</div>

<ng-template #templateName>
    flag 가 true 이면 보임
</ng-template>
```

## `if`(`templateName` 사용), `else`(`templateName` 사용)

```html
<div *ngIf="flag; then thenTemplateName; else elseTemplateName">
    절대 보이지 않음
</div>

<ng-template #thenTemplateName>
    flag 가 true 이면 보임
</ng-template>

<ng-template #elseTemplateName>
    flag 가 false 이면 보임
</ng-template>
```

* 참고
  * [How to use *ngIf else in Angular? - stack overflow](https://stackoverflow.com/questions/43006550/how-to-use-ngif-else-in-angular)