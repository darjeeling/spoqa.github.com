---
layout: entry
title: 유용한 Javascript UI Component 라이브러리 소개
author: 정현석
author-email: hyunsukdn@spoqa.com
description: 웹 애플리케이션을 개발 할 때 유용하고 쉽게 사용 할 수 있는 Javascript UI Component들에 대해 소개 합니다.
publish: true
---


웹 애플리케이션을 개발할 때 기능적으로는 무관하지만, 사용자에게 인터렉티브하고 심미적으로 예쁜 디자인을 제공하고 싶은 경험이 있을 것입니다. 하지만 막상 직접 구현을 하는 것은 생각보다 시간이 오래 걸리고, 구현하더라도 양질의 UI가 나오지 않는 경우들이 있습니다. 그래서 이번 글에서는 쉽고 빠르게 양질의 UI를 제공해주는 라이브러리를 소개해 드리려고 합니다.

#Spin.js
---
  
![list](/images/UI_component/Spinjs.png)
  
작업을 완료하거나 페이지가 넘어갈 때 아무런 말도 없이 그냥 기다리는 경우가 있습니다. 이럴 경우 사용자에게 현재 **기다리는 중**이라는 것을 표현하는 것이 좋습니다. 이러한 기능을 제공해주는 라이브러리가 바로 **[Spin.js]**입니다.  
  
**[Spin.js]**는 위의 그림과 같이 로딩 중이나 무언가를 진행 중이라는 것을 알려주는 사용하기 쉬운 Javascript 라이브러리입니다. 이미지 없이 사용되어 **매우 가볍게 사용**할 수 있습니다. 그리고 사용할 때 쉽게 설정하여 사용할 수 있으며 **대다수 브라우저를 지원**합니다.
  
- [Spin.js] / [Download](https://github.com/fgnass/spin.js)
  
#Datatables
---
  
![list](/images/UI_component/Datatables.png)  
  
많은 양의 정보를 쉽게 볼 수 있도록 **테이블**로 정리해야되는 경우가 있습니다. 그러나 많은 양의 정보를 처리할 때 쉽게 원하는 정보를 찾을 수 있어야 하고 정보가 쉽게 정렬이 될 수 있어야 합니다. 이러한 기능을 제공해주는 라이브러리가 바로 **[Datatables]**입니다.  
  
**[Datatables]**는 위의 그림과 같이 테이블을 **동적인 테이블**을 만들어주는 [JQuery] Javascript 라이브러리입니다. 다양하게 **정렬**할 수 있도록 테이블을 만들수 있으며, 따로 정보를 찾아주는 기능을 만들어주지 않아도 **검색**을 할 수 있는 기능을 제공하고, 정보를 편하게 볼 수 있도록 **구성**을 제공합니다. 그리고 **DOM, Ajax, Server-Side Processing**으로 쉽게 정보를 **[Datatables]**로 만들 수 있습니다.
  
- [Datatables]
  
#Curtain.js
---
  
![list](/images/UI_component/Curtainjs.png)  
  
긴 내용으로 된 하나의 페이지를 섹션별로 효과적으로 **내용을 전환**해야 되는 경우가 있습니다. 그러나 사용자에게 혼란을 주지 않으면서 전환 효과를 만들어 내야 합니다. 이러한 기능을 제공해주는 라이브러리가 바로 **[Curtain.js]**입니다.  
  
**[Curtain.js]**는 위의 그림과 같이 마치 **커튼**이 걷히는 것처럼 내용 **전환 효과**를 주는 [JQuery] Javascript 라이브러리입니다. 각 내용을 화면에 고정하고 스크롤이나 키보드를 통해 화면을 전환하여 트렌디하면서 인터렉티브한 느낌을 쉽게 제공할 수 있습니다.
  
- [Curtain.js] / [Download](https://github.com/Victa/curtain.js)
  
#Turn.js
---
  
![list](/images/UI_component/Turnjs.png)  
  
위의 [Curtain.js]가 세로형태의 전환 효과를 내는 것이었다면 **가로형태의 전환 효과**를 내야 하는 경우가 있습니다. 이러한 기능을 제공해주는 라이브러리가 바로 **[Turn.js]**입니다.  
  
**[Turn.js]**는 위의 그림과 같이 책장을 넘기는 듯한 내용 전환 효과를 주는 [JQuery] Javascript 라이브러리입니다. 하나에 페이지를 섹션별로 나눠서 키보드를 통해 화면을 전환하여 책장을 넘기는 느낌을 제공해 스마트폰이나 태블릿에서 **책을 읽는 듯한 느낌**을 쉽게 제공할 수 있습니다.
  
- [Turn.js] / [Download](https://github.com/blasten/turn.js)
  
#Glfx.js
---
  
![list](/images/UI_component/Glfxjs.png)  
  
이미지를 따로 수정해서 올리는 것이 아니라 웹에서 바로 밝기를 조정하거나 다양한 **효과**를 주고 싶은 때도 있습니다. 이러한 기능을 제공해주는 라이브러리가 바로 **[Glfx.js]**입니다.
  
**[Glfx.js]**는 위의 그림과 같이 다양한 효과를 주는 [WebGL]기반의 Javascript 라이브러리입니다. 이미지에 Blur 효과, 세피아, 밝기 조절, 모자이크처리 등 **다양한 효과**를 다양한 설정을 통해 쉽게 사용 할 수 있습니다. 그러나 [WebGL] 기반으로 되어 있어서 [WebGL을 지원하는 브라우저](http://www.khronos.org/webgl/wiki/Getting_a_WebGL_Implementation)만 가능합니다.
  
- [Glfx.js] / [Download](https://github.com/evanw/glfx.js)
  
#JQuery Tag-it
---
  
![list](/images/UI_component/Tag-it.png)  
  
태그를 넣을 때  쉽게 수정 가능하게 하고 자동완성기능을 넣고 싶은 때도 있습니다. 이러한 기능을 제공해주는 라이브러리가 바로 **[JQuery Tag-it]**입니다.  
  
**[JQuery Tag-it]**은 위의 그림과 같이 태그에 대한 [JQuery] Javascript 라이브러리입니다. **쉽게 태그를 넣고 지울** 수 있으며 태그에 대해 자동완성 기능을 지원합니다. 그리고 각 태그에 대해 이벤트를 줄 수 있어서 매우 유용하게 사용하실 수 있습니다.
  
- [JQuery Tag-it] / [Download](https://github.com/aehlke/tag-it)
  
#Tinycon
---
  
![list](/images/UI_component/Tinycon.png)  
  
새 글의 개수나 접속자 수에 대한 정보를 사용자에게 알리고 싶은 때도 있습니다. 이럴 경우 브라우저 **탭에 정보를 제공**하는 경우가 있습니다. 이러한 기능을 제공해주는 라이브러리가 바로 **[Tinycon]**입니다.
  
**[Tinycon]**는 위의 그림과 같이 파비콘에 **동적인 숫자**를 통해 정보를 알리는 Javascript 라이브러리입니다. 매우 쉽게 사용할 수 있으며, 설정을 통해 어떤 내용을 숫자로 표현할 것인지를 쉽게 사용자화 할 수 있습니다. 파비콘에 경우 브라우저 탭에 항상 보이기 때문에 아주 유용하게 사용할 수 있을 것 같습니다. 그러나 **현재 크롬, 파이어폭스, 오페라 브라우저**만이 지원 가능합니다.
  
- [Tinycon] / [Download](https://github.com/tommoor/tinycon)
  
#3D GALLERY
---
  
![list](/images/UI_component/3DGallery.png)  
  
사진이나 슬라이드 탭을 보여주기 위해 **갤러리 공간**을 만듭니다. 그래서 좀 더 효과적으로 보여주기 위해 다양한 효과를 넣는 경우가 있습니다. 이러한 기능을 제공해주는 라이브러리가 바로 **[3D GALLERY]**입니다.  
  
**[3D GALLERY]**는 위의 그림과 같이 내용을 3D로 나열해 보여주는 [JQuery] Javascript 라이브러리입니다. 간단한 설정으로 **3D로 배치하고 움직**이도록 할 수 있습니다. 그리고 자동으로 내용을 넘어가게 할 수도 있고 다양하게 바뀌는 효과를 줄 수 있습니다.
  
- [3D GALLERY] / [Demo](http://tympanus.net/Development/3DGallery/)
  
#글을 마치면서
---
이번 글에서는 UI Component Javascript 라이브러리들에 대해 알아봤습니다. 위의 라이브러리로 좀 더 쉽고 빠르게 양질의 웹 애플리케이션을 개발할 수 있었으면 좋겠습니다.


[Spin.js]: http://fgnass.github.com/spin.js/
[Datatables]: http://datatables.net/
[Curtain.js]: http://curtain.victorcoulon.fr/
[Turn.js]: http://www.turnjs.com/
[Glfx.js]: http://evanw.github.com/glfx.js/
[JQuery Tag-it]: http://aehlke.github.com/tag-it/
[Tinycon]: http://tommoor.github.com/tinycon/
[3D GALLERY]: http://tympanus.net/codrops/2012/02/06/3d-gallery-with-css3-and-jquery/
[WebGL]: http://www.khronos.org/webgl/
[JQuery]: http://jquery.com/