---
layout: post
title: 'GDG WebTech Workshop: 측정하는 놈, 로딩하는 놈, 그리는 놈'
date: 2016-09-16 20:30:00
categories: Review
tags: [frontend, gdg, gdg webtech]
published: true
fullview: false
comments: true
---

{% include toc.html %}

지난 8월 27일 GDG WebTech에서 진행하는 [Workshop](http://onoffmix.com/event/75765)이 있어 참여하게 되었다. 이 Workshop은 프론트엔드 성능 최적화에 대한 내용으로 초/중급 프론트엔드 개발자를 대상으로 진행되었다.

진행되었던 주제와 슬라이드는 아래와 같다.

* 로딩 성능 병목 구간, 인스턴트 로딩에서 오프라인 지원까지
* 크롬의 렌더링 성능 인자에 대한 이해
* 성능 측정을 위한 도구들

1. http://www.slideshare.net/cwdoh/gdg-webtech-1
2. http://www.slideshare.net/cwdoh/instant-and-offline-apps-with-service-worker
3. http://www.slideshare.net/cwdoh/service-worker-101
4. http://www.slideshare.net/cwdoh/overview-how-to-measure-your-web-app

이번 Workshop에서는 프론트엔드 성능 최적화에 대한 여러 내용들이 있었지만, 그중 가장 관심이 있던 부분이었던 부분만 아래와 같이 정리하였다. Service Worker는 기회가 된다면 추후 정리해볼 생각이다.

## 프론트엔드 개발자를 위한 크롬 렌더링 성능 인자 이해하기

<iframe src="//www.slideshare.net/slideshow/embed_code/key/gB5gPyQK2Z5iyG" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/cwdoh/gdg-webtech-1" title="프론트엔드 개발자를 위한 크롬 렌더링 성능 인자 이해하기" target="_blank">프론트엔드 개발자를 위한 크롬 렌더링 성능 인자 이해하기</a> </strong> from <strong><a href="//www.slideshare.net/cwdoh" target="_blank">Chang W. Doh</a></strong> </div>

### 그래픽스 & 하드웨어 가속

![S/W 렌더링과 H/W Acc. 레더링](http://image.slidesharecdn.com/gdgwebtech1-2-140619105952-phpapp02/95/-8-638.jpg?cb=1407742521)

![Software only](http://image.slidesharecdn.com/gdgwebtech1-2-140619105952-phpapp02/95/-13-638.jpg?cb=1407742521)

![GPU Acceleration](http://image.slidesharecdn.com/gdgwebtech1-2-140619105952-phpapp02/95/-14-638.jpg?cb=1407742521)

### CPU와 GPU 사이에 존재하는 이슈들

* 서로 다른 메모리 공간
* 메로리는 한계가 있다.
* 데이터는 자주 변경된다.

### GPU가 잘하는 것

**"GPU는 수신된 데이터로 무언가를 그리는데 적합"**

1. 텍스쳐를 가지고 이미지를 빠르게 출력 가능
2. 이미 가진 텍스쳐는 다시 받지 않고 재활용
3. 회전, 확대 , 축소, 기울임, 반투명 처리 등
4. 이 기능들을 동시 처리하는 것도 매우 최적화

### GPU가 못하는 것(이슈)

#### 비디오 메모리로의 데이터 전송 속도

**"비디오 메모리로의 데이터 전송 속도 느림"**
데이터 전송 시간 = 데이터의 크기 / BUS 속도

* 일반적으로 예상되는 데이터 크기:
    * GPU 명령 < 버텍스 < 텍스터 이미지(화면에서 보는게 모든게 이미지)

#### CPU 처리 시간

GPU의 데이터는 CPU에서 생성 후 전송(GPU가 그리는 그림은 CPU가 그려줌)

### 렌더링 선능의 주요 인자

* GPU는 회전/확대/축소/기울임/반투명 처리 등에 최적화
	* 이 범주의 기능으로 렌더링이 처리될 수 있도록
* GPU에 사용할 데이터를 준비하는 것은 CPU의 몫
	* CPU가 새로운 데이터를 만드는 작업은 최소화
* CPU가 준비한 데이터는 비디오 메모리에 전송 필요
	* 데이터 전송을 최소화할 수 있도록

### 크롬의 렌더링

1. 웹 페이지는 파싱을 통해 DOM 트리로 해석되어 메모리에 적재
2. DOM 트리를 렌더링 트리로 생성 후 각 노드들을 개별적인 이미지로 생성
3. 트리 구조 및 스타일에 따라 이미지를 배치 합성하여 출력

### 이슈: Reflow

* Reflow = Layout = Layouting
	* DOM 노드가 가지는 레이아웃 정보가 변경되면 레이아웃은 재배치를 위한 계산이 필요

#### Reflow로 발생할 수 있는 일

1. 레이아웃의 변경이 트리를 따라 전파(CPU)
2. 많은 경우 레이어 이미지의 갱신 필요(CPU)
3. 레이어 이미지가 변경되면 VRAM의 텍스쳐 갱신 필요(RAM to VRAM Bandwidth)

### 이슈: Repaint

* Repaint = Redraw
	* 레이아웃 내 컨텐츠가 변경 시 텍스쳐를 새로 생성 필요

#### Reflow/Repaint 발생 요인

* DOM 노드의 동적인 추가/삭제/업데이트
* DOM 노드의 감춤/표시
	* diplay: none
	* visibility: hidden
* DOM 노드의 이동 애니메이션
* 스타일시트의 추가 혹은 스타일 속성의 변경
	* 미디어 쿼리 역시
* 브라우저 사이즈 변경
* 폰트 변경
* 스크롤
* ...

### 정리: 크롬에서의 전반적인 렌더링 흐름

1. DOM으로부터 노드들을 개별적으로 혹은 그룹 지어 레이어 단위들로 분리
2. 레이아웃을 계산하고 각 레이어들이 그려져야 할 영역의 크기 위치 등을 계산
	* 위치/크기 정보 등을 계산하기 위한 CPU의 계산 오버헤드가 발생
3. 레이어들 각각은 렌더링을 위해 비트맵으로 출력
	* CPU에서 레이어 이미지를 생성하는 오버헤드가 발생
4. 생성된 비트맵을 GPU에 테긋쳐로 업로드
	* GPU의 비디오 메모리로 전송하는 오버헤드는 발생
5. 계산된 레이아웃으로 레이어의 텍스쳐 이미지들을 최종 스크린 이미지로 합성

### 렌더링 성능 최적화

* 렌더링 패스는 철저하게 브라우저의 영역
* 병목 구간의 발생 요인을 피해갈 것
* 피해야 할 성능의 위험 인자
	* CPU에서 텍스쳐 이미지를 생성하는 요인들
	* 레이아웃 변경의 요인

### 가장 간단한 Hack: 레이어의 분리

**"크롬에서 DOM 노드가 레이어로 분리되는 조건들"**

1. 3D 혹은 Perspective를 표현하는 CSS transform 속성을 가진 경우
2. 하드웨어 가속 디코딩을 사용하는 `<video>` 엘리먼트
3. 3D 컨텍스트 혹은 하드웨어 가속 2D 컨텍스트를 가지는 `<canvas>` 엘리먼트
4. (플래시와 같은) 플러그인 영역
5. 투명도(opacity) 속성 혹은 transform 애니메이션의 사용
6. 가속 가능한 CSS 필터를 가진 경우
7. 낮은 z-index를 가진 형제 노드가 Compositing Layer를 가진 경우

**요약: 해당 DOM 노드가 주변 노드와는 별도로 렌더링되어야 빠른 경우**

> `translateZ(0)`
>
> * 노드의 Z축 값으로 0을 주는 무의미한 코드
> * 첫번째 항목에 해당

### 강제적인 레이어 분리가 만능은 아니다.

* 레이어 분리는 필연적으로 텍스쳐 이미지 분리를 의미
	* 추가적인 메모리 소모
* 메모리는 유한하다.
	* 메모리 공간 부족 시 기존 데이터 릴리즈 후 새로운 데이터의 업로드
	* 레이어 분리를 통한 성능 이점을 송수신 오버헤드로 상쇄

**따라서, 레이어 분리는 최소화 필요**

> 하드웨어 가속으로 얻는 성능은 절대로 공짜가 아니며, 모든 것에 가능성을 두고 확인해야 한다.