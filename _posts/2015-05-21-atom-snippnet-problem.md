---
layout: post
title: 'Atom에서 autocomplete-plus를 사용 중 tab과 enter가 작동하지 않는 경우'
date: 2015-05-21 02:54:00
categories: Program
tags: [Atom, autocomplete-plus]
published: true
---

[관련 글]({% post_url 2015-05-21-atom %})에서 Atom에 대해 글을 작성했었다. 설치하고 사용중에 autocomlete을 사용할 때 tab이나 enter가 정상적으로 작동하지 않는 문제가 생겼다. 정확한 원인을 찾지 못해 google을 검색하던 도중에 해결 방법을 찾았다.

필자가 겪은 문제는 추가로 설치한 emmet과 autocomplete-plus가 충돌이 나는 경우였다. 필자는 [이 글](https://github.com/emmetio/emmet-atom/issues/146#issuecomment-103475628)에서 해결법을 찾을 수 있었다.

* 메뉴에서 atom을 누른다.(Mac OS에 해당하며 Windows는 File을 누른다.)
* Open Your Keymap을 선택한다.
* 아래의 코드를 ``keymap.cson``에 붙여넣어 ``enter``에 대한 기능을 overwrite한다.

```
'atom-text-editor:not(mini).autocomplete-active':
    'enter': 'autocomplete-plus:confirm'
```

위 작업을 수행하면 emmet 플러그인은 ``tab``으로 사용할 수 있으며, built-in 플러그인은 ``enter``을 통하여 사용할 수 있다.


