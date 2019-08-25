---
layout: post
title: '[Openlayers] Render Event 정리'
date: 2018-10-23 17:00:00
categories: map
tags: [openlayers]
published: true
fullview: false
comments: true
---

## 이벤트

### [Map](http://openlayers.org/en/latest/apidoc/module-ol_Map-Map.html)

<div class="overflow-wrapper" markdown="block">

| event | module | note |
| :--: | -- | -- |
| [postcompose](http://openlayers.org/en/latest/apidoc/module-ol_render_Event-RenderEvent.html#event:postcompose) | [module:ol/render/Event~RenderEvent](http://openlayers.org/en/latest/apidoc/module-ol_render_Event-RenderEvent.html) |  |
| [postrender](http://openlayers.org/en/latest/apidoc/module-ol_MapEvent-MapEvent.html#event:postrender) | [module:ol/MapEvent~MapEvent](http://openlayers.org/en/latest/apidoc/module-ol_MapEvent-MapEvent.html) | Triggered after a map frame is rendered. |
| [precompose](http://openlayers.org/en/latest/apidoc/module-ol_render_Event-RenderEvent.html#event:precompose) | [module:ol/render/Event~RenderEvent](http://openlayers.org/en/latest/apidoc/module-ol_render_Event-RenderEvent.html) |  |

</div>

### [PluggableMap](http://openlayers.org/en/latest/apidoc/module-ol_PluggableMap-PluggableMap.html)

<div class="overflow-wrapper" markdown="block">

| event | module | note |
| :--: | -- | -- |
| [postcompose](http://openlayers.org/en/latest/apidoc/module-ol_render_Event-RenderEvent.html#event:postcompose) | [module:ol/render/Event~RenderEvent](http://openlayers.org/en/latest/apidoc/module-ol_render_Event-RenderEvent.html) |  |
| [postrender](http://openlayers.org/en/latest/apidoc/module-ol_MapEvent-MapEvent.html#event:postrender) | [module:ol/MapEvent~MapEvent](http://openlayers.org/en/latest/apidoc/module-ol_MapEvent-MapEvent.html) | Triggered after a map frame is rendered. |
| [precompose](http://openlayers.org/en/latest/apidoc/module-ol_render_Event-RenderEvent.html#event:precompose) | [module:ol/render/Event~RenderEvent](http://openlayers.org/en/latest/apidoc/module-ol_render_Event-RenderEvent.html) |  |
| [rendercomplete](http://openlayers.org/en/latest/apidoc/module-ol_render_Event-RenderEvent.html#event:rendercomplete) | [module:ol/render/Event~RenderEvent](http://openlayers.org/en/latest/apidoc/module-ol_render_Event-RenderEvent.html) | Triggered when rendering is complete, i.e. all sources and tiles have finished loading for the current viewport, and all tiles are faded in. |

</div>

### [WebGLMap](http://openlayers.org/en/latest/apidoc/module-ol_WebGLMap-WebGLMap.html)

<div class="overflow-wrapper" markdown="block">

| event | module | note |
| :--: | -- | -- |
| [postcompose](http://openlayers.org/en/latest/apidoc/module-ol_render_Event-RenderEvent.html#event:postcompose) | [module:ol/render/Event~RenderEvent](http://openlayers.org/en/latest/apidoc/module-ol_render_Event-RenderEvent.html) |  |
| [postrender](http://openlayers.org/en/latest/apidoc/module-ol_MapEvent-MapEvent.html#event:postrender) | [module:ol/MapEvent~MapEvent](http://openlayers.org/en/latest/apidoc/module-ol_MapEvent-MapEvent.html) | Triggered after a map frame is rendered. |
| [precompose](http://openlayers.org/en/latest/apidoc/module-ol_render_Event-RenderEvent.html#event:precompose) | [module:ol/render/Event~RenderEvent](http://openlayers.org/en/latest/apidoc/module-ol_render_Event-RenderEvent.html) |  |

</div>

### [Layer](http://openlayers.org/en/latest/apidoc/module-ol_layer_Layer-Layer.html)

<div class="overflow-wrapper" markdown="block">

| event | module | note |
| :--: | -- | -- |
| [postcompose](http://openlayers.org/en/latest/apidoc/module-ol_render_Event-RenderEvent.html#event:postcompose) | [module:ol/render/Event~RenderEvent](http://openlayers.org/en/latest/apidoc/module-ol_render_Event-RenderEvent.html) |  |
| [render](http://openlayers.org/en/latest/apidoc/module-ol_MapEvent-MapEvent.html#event:postrender) | [module:ol/MapEvent~MapEvent](http://openlayers.org/en/latest/apidoc/module-ol_MapEvent-MapEvent.html) |  |
| [precompose](http://openlayers.org/en/latest/apidoc/module-ol_render_Event-RenderEvent.html#event:precompose) | [module:ol/render/Event~RenderEvent](http://openlayers.org/en/latest/apidoc/module-ol_render_Event-RenderEvent.html) |  |
| [rendercomplete](http://openlayers.org/en/latest/apidoc/module-ol_render_Event-RenderEvent.html#event:rendercomplete) | [module:ol/render/Event~RenderEvent](http://openlayers.org/en/latest/apidoc/module-ol_render_Event-RenderEvent.html) | Triggered when rendering is complete, i.e. all sources and tiles have finished loading for the current viewport, and all tiles are faded in. |

</div>

### [ImageLayer](http://openlayers.org/en/latest/apidoc/module-ol_layer_Layer-Layer.html)

<div class="overflow-wrapper" markdown="block">

| event | module | note |
| :--: | -- | -- |
| [postcompose](http://openlayers.org/en/latest/apidoc/module-ol_render_Event-RenderEvent.html#event:postcompose) | [module:ol/render/Event~RenderEvent](http://openlayers.org/en/latest/apidoc/module-ol_render_Event-RenderEvent.html) |  |
| [ender](http://openlayers.org/en/latest/apidoc/module-ol_MapEvent-MapEvent.html#event:postrender) | [module:ol/MapEvent~MapEvent](http://openlayers.org/en/latest/apidoc/module-ol_MapEvent-MapEvent.html) |  |
| [precompose](http://openlayers.org/en/latest/apidoc/module-ol_render_Event-RenderEvent.html#event:precompose) | [module:ol/render/Event~RenderEvent](http://openlayers.org/en/latest/apidoc/module-ol_render_Event-RenderEvent.html) |  |
| [rendercomplete](http://openlayers.org/en/latest/apidoc/module-ol_render_Event-RenderEvent.html#event:rendercomplete) | [module:ol/render/Event~RenderEvent](http://openlayers.org/en/latest/apidoc/module-ol_render_Event-RenderEvent.html) | Triggered when rendering is complete, i.e. all sources and tiles have finished loading for the current viewport, and all tiles are faded in. |

</div>

### [Heatmap](http://openlayers.org/en/latest/apidoc/module-ol_layer_Heatmap-Heatmap.html)

<div class="overflow-wrapper" markdown="block">

| event | module | note |
| :--: | -- | -- |
| [postcompose](http://openlayers.org/en/latest/apidoc/module-ol_render_Event-RenderEvent.html#event:postcompose) | [module:ol/render/Event~RenderEvent](http://openlayers.org/en/latest/apidoc/module-ol_render_Event-RenderEvent.html) |  |
| [render](http://openlayers.org/en/latest/apidoc/module-ol_MapEvent-MapEvent.html#event:postrender) | [module:ol/MapEvent~MapEvent](http://openlayers.org/en/latest/apidoc/module-ol_MapEvent-MapEvent.html) |  |
| [precompose](http://openlayers.org/en/latest/apidoc/module-ol_render_Event-RenderEvent.html#event:precompose) | [module:ol/render/Event~RenderEvent](http://openlayers.org/en/latest/apidoc/module-ol_render_Event-RenderEvent.html) |  |
| [rendercomplete](http://openlayers.org/en/latest/apidoc/module-ol_render_Event-RenderEvent.html#event:rendercomplete) | [module:ol/render/Event~RenderEvent](http://openlayers.org/en/latest/apidoc/module-ol_render_Event-RenderEvent.html) | Triggered when rendering is complete, i.e. all sources and tiles have finished loading for the current viewport, and all tiles are faded in. |

</div>

## 정리

위 이벤트들의 작동을 확인하기 위해 [Openlayers render event](/playground/examples/openlayers-render-event.html)라는 예제 페이지를 작성하여 확인하였다.

<figure><img src="/images/map/openlayers/openlayers-render-event.png" alt=""></figure>

* `Map`은 `precompose`가 발생 이후, `Layer`들의 사이클이 돈 이후, `postcompose`, `postrender` 발생
* 하나의 `Layer`가 사이클이 돌 땐, `precompose` ~ `postcompose`의 일련의 과정이 발생하고 다음 `Layer`로 이동
* 타일 `Layer`는 불리는 대로 일련의 과정이 발생하고, `precompose`, `postcompose`만 발생
* `VectorLayer`는 `precompose`, `render`, `postcompose`가 발생
* `VectorLayer`의 `style function`은 `precompose` 전에 실행
* `VectorLayer`의 `style function`은 갱신의 필요가 없을 경우 발생하지 않음(미세하게 지도가 움직이거나 새로운 레이어가 추가되었을 경우)
* `Layer`의 `rendercomplete`가 발생하는 경우는 확인하지 못했으며, `Map`의 `rendercomplete`는 최초 로드 시를 제외하고 가장 마지막 `postrender` 이후 발생
