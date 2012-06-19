---
layout: entry
title: 웹 퍼포먼스 높이기
author: akaz
author-email: akaz@spoqa.com
description: 웹 퍼포먼스를 높일 수 있는 몇가지 팁에 대해 소개합니다.
---

해외 블로그 포스팅을 보던 중 웹 어플리케이션의 퍼포먼스와 사용자 경험을 높일 수 있는 여러가지의 팁을 알아낼 기회가 생겨, 공유하고자 합니다. 곧바로 적용할 수 있을만한 간단한 팁들 위주로 내용을 써보았습니다. 

해당 정보를 얻어낸 원본 사이트의 링크를 밑에 첨부해두었으니 영어를 읽는 것에 거부감 없으신 분들은 링크를 따라가 보는 것을 추천합니다.

#### CSS Transition 속성 사용
---

css의 많은 속성들을 변경시에 [transition이펙트](http://www.w3schools.com/css3/css3_transitions.asp)를 사용할 수 있습니다. transition속성을 이용하여 두 상태를 전이시킬 수 있는데, 대부분의 css속성들을 이용할 수 있습니다. 가령 text-shadow가 점점 더 커진다던지 스무스하게 배경 색이 바뀐다던지 하는 효과를 보여줄 수 있습니다. 당연히 엘리먼트의 위치나 크기 이동도 가능합니다. 실제 적용 코드를 보게 되면

{% highlight css %}
div.box {
    left: 40px;
    -webkit-transition: all 0.3s ease-out;
    -moz-transition: all 0.3s ease-out;
    -o-transition: all 0.3s ease-out;
    transition: all 0.3s ease-out;
}
div.box.totheleft { 
    left: 0px; 
}
div.box.totheright { 
    left: 80px; 
}
{% endhighlight %}

가령 위의 코드의 경우 box클래스를 가진 div엘리먼트에 jQuery의 addClass같은 메서드를 사용하여 totheleft, totheright 클래스를 추가하면 왼쪽, 오른쪽 40px씩 이동하는 효과를 보여줍니다. 각종 자바스크립트 라이브러리의 애니메이션 효과를 이용하는 것보다 깔끔하게 애니메이션 효과를 구현할 수 있다는 장점 외에도 transition이펙트 사용시 하드웨어 가속 효과를 적용받기 때문에 더 부드럽게 표시되는 효과를 얻을 수 있습니다.

#### 자바스크립트 배열에 추가된 메서드 사용
---

자바스크립트 버전이 오르면서 새로 추가된 [배열의 메서드](https://developer.mozilla.org/en/JavaScript/Reference/Global_Objects/Array#Methods)들을 이용하면 성능 향상을 기대할 수 있습니다. 가령 filter, forEach, every같은 배열의 메서드는 배열의 모든 원소에 적용할 함수를 인자로 받는데

{% highlight javascript %}
for (var i = 0, len = arr.length; i < len; i++)
{% endhighlight %}

위와 같이 for구문을 이용하여 이 원소들을 다루는 것보다 해당 함수를 이용하여 원소를 조작하는 것이 더 빠릅니다. 따라서 자바스크립트 버전만 따라준다면 이러한 메서드를 이용하는 것이 좋습니다.

#### HTML5의 새로운 input type들을 이용하기
---

웹 퍼포먼스의 속도 측면과는 상관없지만 사용자 경험 측면에서 좀 더 나은 경험을 제공해주고 싶다면 html5에서 새롭게 추가된 input태그의 속성들을 눈여겨 볼 필요가 있습니다. input태그의 type속성에서 늘상 쓰이던 text, password, file 이외에도 search, tel, url, email, datetime, date, month, week, time, datetime-local, number, range 그리고 color와 같은 속성을 input type에 적용할 수 있습니다.

이러한 input필드들에 포커스가 갔을 경우에 취해질 행동이 벤더마다 다르게 구현되어있을 수 있지만(가령 color를 써놓으면 colorpicker를 띄워준다던가), 만약에 브라우저 벤더에서 적절한 구현을 해놓지 않았다고 하더라도 무시하고 디폴트 액션을 수행하므로 걱정 없이 쓸 수 있습니다.

또한 [placeholder속성](http://www.w3schools.com/html5/att_input_placeholder.asp)을 지정하면 아무런 입력이 없는 input필드에 디폴트 스트링을 넣어주게 됩니다. [autofocus속성](http://www.w3schools.com/html5/att_input_autofocus.asp)은 페이지가 로드 되고 자동으로 그 필드에 포커스가 이동하도록 하고, [required속성](http://www.w3schools.com/html5/att_input_required.asp)을 input의 속성으로 달아놓고 required값을 주게 되면 그 input필드를 채우지 않으면 submit을 하지 않도록 합니다.

{% highlight html %}
<input type="text" required="required" />
{% endhighlight %}

즉, 필수로 입력해야 할 필드가 있다면 스크립트로 별다른 조취를 취하지 않고 required옵션을 이용하면 편합니다. 
마지막으로 [pattern속성](http://www.w3schools.com/html5/att_input_pattern.asp)이 있습니다. pattern에 [정규 표현식](http://ko.wikipedia.org/wiki/%EC%A0%95%EA%B7%9C_%ED%91%9C%ED%98%84%EC%8B%9D)을 써넣으면 그 정규표현식으로 입력값을 필터링하여 만약 올바르지 않은 입력값이 들어올 경우 submit을 하지 않도록 합니다.

{% highlight html %}
<input type="text" name="country_code" pattern="[A-Za-z]{3}" />
{% endhighlight %}

가령 위의 input태그는 3자리의 대, 소문자 알파벳으로 이루어진 문자열만을 입력값으로 받아들인다는 것을 알 수 있습니다.

단, 이 많은 속성들중 브라우저에서 지원되지 않는 속성들이 있습니다. 가령 ie의 경우 placeholder속성을 지원하지 않는데 다른 방법을 찾아야 합니다. (가령 [placeholder를 js를 이용한 구현한 예](https://github.com/NV/placeholder.js/)가 있습니다.) 현실적으로 대부분의 브라우저에 대응해야 하는 데스크탑용 사이트에서는 이러한 속성을 사용하기가 힘들테지만 webkit엔진에서는 대부분의 새로운 html5기능들을 지원하기 때문에 모바일용 웹 어플리케이션 작성시 이러한 속성들이 유용하게 쓰일 수 있을거라 생각합니다.

#### CSS3의 새로운 속성들을 적극 이용하기
---

기존에는 CSS에서 적용할 수 있는 속성들이 제한적이어서 재미있는 효과를 주기가 힘들었습니다. 따라서 이미지를 직접 포토샵같은 프로그램으로 보정해서 img태그를 이용하여 보여주어야 했습니다. 이미지를 편집하는데 들어가는 노력도 노력이지만 이미지는 대체적으로 CSS를 이용하는것보다 용량을 더차지한다는 문제도 있었습니다. 하지만 CSS3부터 여러 재미있는 이펙트를 추가할수 있게 되었습니다. 가령 이러한 것들이 가능합니다.

* [그라디언트](http://gradients.glrzad.com/)
* [border-radius](http://border-radius.com/)
* [box-shadow](http://www.css3.info/preview/box-shadow/)
* [RGBA](http://css-tricks.com/rgba-browser-support/)
* [변형 & 회전](http://davidwalsh.name/css-transform-rotate)
* [CSS Masks](http://www.webkit.org/blog/181/css-masks/)

이 있습니다. 이러한 효과들을 이용하여 [재미있는 효과](http://www.phpied.com/css-performance-ui-with-fewer-images/)들을 줄 수 있습니다. CSS3를 이용하지 못하는 구식 브라우저 사용자들을 지원해야 하는 문제가 있지만 이러한 속성 미지원시 이미지로 대체하는 방식을 쓰면 됩니다. [Modernizr](http://modernizr.com/)를 이용하면 이러한 문제를 해결하는데 큰 도움이 될 수 있다는군요.

#### 터치 이벤트 관련 팁
---

모바일 환경에서 웹뷰를 이용한 터치 이벤트를 구현할 시에 [click이벤트](http://api.jquery.com/click/)를 등록하는 경우가 있습니다. 그런데 엘리먼트에 click이벤트를 걸었을 경우 실제 엘리먼트를 탭하면 300ms이내의 딜레이가 발생하게 됩니다. 딜레이가 발생하는 이유는 탭 이후 이어질 수 있는 액션들(가령 더블탭)을 판단하기 위함인데요, 실질적으로 이러한 판단이 필요 없을 경우가 많습니다. 그래서 대안으로 touchstart, touchmove, touchend등의 이벤트를 등록할 수 있는데요, [구글에서 제안하는 fast button 구현 방법](https://developers.google.com/mobile/articles/fast_buttons)에 대한 글에서 해당 내용을 찾아볼 수 있습니다. iOS의 경우 애플에서 제공한 [Safari 이벤트 핸들링 방식에 대한 페이지](http://developer.apple.com/library/ios/#DOCUMENTATION/AppleApplications/Reference/SafariWebContent/HandlingEvents/HandlingEvents.html)에서 비슷한 내용을 찾아 볼 수 있습니다. 

다시 본론으로 돌아와서 딜레이를 없애주기 위해 엘리먼트의 click이벤트를 삭제해주어야 합니다. 

{% highlight javascript %}
$('a').live('click', function(e) {
  e.preventDefault();
  return false;
})
{% endhighlight %}

원문 블로그에서는 [Zepto](http://zeptojs.com/)라는 자바스크립트 라이브러리에 tap이벤트를 소개하였는데요, tap이벤트는 모바일 환경에서의 탭 이벤트를 받도록 정의된 이벤트입니다. 따라서 zepto를 이용한다면 간단하게 tap이벤트를 구현할 핸들러를 등록할 수 있습니다. jQuery와 똑같은 인터페이스를 제공하기 때문에 어려움 없이 등록 가능 합니다.

{% highlight javascript %}
$('a').live('tap', handleTaps);
{% endhighlight %}

단, 주의해야 할 점이 있습니다. click이벤트를 명시적으로 제거해버렸기 때문에 해당 엘리먼트가 가지고 있는 디폴트 행동을 복구시켜야 합니다. 가령 앵커 태그의 경우 window.location.href에 해당 앵커 태그가 가지고 있는 url을 직접 대입해 주는 방식등을 사용하여 기본 행동을 재작성해주어야 하는 약간의 불편함이 있습니다.

{% highlight javascript %}
function handleTaps( event ) {
  /* 필요한 로직 수행 후 */

  // 그리고 실제로 이루어져야 할 기본 엘리먼트 반응을 다시 복구 시킬 수 있어야 합니다. 
  // 물론 이러한 일이 필요없을 경우에는 구지 하지 않아도 됩니다.
  window.location.href = $(event.currentTarget).attr('href')
}
{% endhighlight %}

앞서 소개한 zepto는 모바일 페이지에 최적화되어 만들어진 jQuery 대응용 라이브러리인데, 용량도 적고, jQuery와 비슷한 인터페이스를 가지고 있으며 ie같이 현재로서는 모바일 웹과 별 상관 없는 브라우저 지원을 뺀 컴팩트한 라이브러리이므로 모바일 웹뷰를 위한 페이지를 제작하고 있다면 지금 적용해보는 것을 추천합니다. 

그 외에도 모바일 페이지에 필요할만한 터치 관련 이벤트들이 여러개 존재하니 [터치 이벤트 페이지](http://zeptojs.com/#Touch events)를 참조하시면 좋을 것 같습니다.

#### 클라이언트 사이트 템플레이트
---

모바일 환경에서는 반응 속도가 매우 중요하기 때문에, 가급적 네트워크를 통한 요청 시 받아오는 양이 줄어들도록 하는 것이 중요합니다. 그런데 서버로 부터 요청시마다 html문서를 가져오게 되면 아무래도 json객체를 가져오는 방법보단 통신량이 많아지게 됩니다. 따라서 \<script\>tag에 필요한 html문서 구조를 미리 기록해 놓았다가 필요할 시에 jQuery의 [clone메서드](http://api.jquery.com/clone/) 등을 이용하여 필요한 내용만 채워넣는 방식으로 뷰를 구성하는 것이 더 합당할 수 있습니다. 

데스크탑 환경이라면 html을 전달받는 정도는 정말 미미한 정도의 양이고 통신 환경이 쾌적하기 때문에 이 정도로 극단적(?)인 시도를 할 필요는 없어보입니다만 모바일일 경우 웹뷰에서 보이는 것이 네이티브 어플리케이션 속도를 따라갈 수 있기를 바랄 것이므로 이렇게 퍼포먼스를 향상하는 것도 방법이 될 수 있다고 생각됩니다.

블로그에서는 템플레이팅용 라이브러리로 [mustache](http://mustache.github.com/)를 소개하고 있는데 실제 사용 [데모](http://mustache.github.com/#demo)를 보면 json객체를 이용한 템플레이팅에 최적화된 기능을 보여주는 것을 알 수 있습니다. 

조금이나마 더 웹페이지의 속도를 향상시키는데에 관심이 있는 분들은 적용을 고려해봄이 어떨까 싶습니다. 

#### 빠른 이미지 변경
---

{% highlight css %}
input { background-image: url('images/text.png') }
input:focus { background-image: url('images/text-focus.png') }
{% endhighlight %}

엘리먼트의 상태가 바뀔때마다 다른 이미지를 로딩하고 싶을 경우가 있습니다. 위의 코드를 보면 포커스가 되었을 때 기존의 배경 이미지인 text.png파일을 text-focus.png파일로 바꾸어주는 것을 알 수 있습니다. 그런데 이렇게 파일을 따로따로 로딩하는 방식을 이용하는 것보다 한 이미지 파일에 두 이미지를 넣어놓고 [background-position](http://www.w3schools.com/cssref/pr_background-position.asp)같은 속성을 이용하는 것이 더 빠릅니다.

{% highlight css %}
.sprite { 
  background-image: url(sprite.png);
  background-repeat:no-repeat;
}
.sprite.text { background-position:0px 0px; }
.sprite.text:focus { background-position:0px -40px; }
{% endhighlight %}

위의 코드의 경우가 그러한 방법을 적용한 예라고 볼 수 있습니다. [background-image](http://www.w3schools.com/cssref/pr_background-image.asp)에 변이될 이미지 2개를 넣어놓고 상태가 바뀔때마다 background-position속성을 바꾸는 것을 알 수 있습니다. 이 방법은 사실 옛날 2D게임에서 [스프라이트](http://en.wikipedia.org/wiki/Sprite_(computer_graphics\))를 이용하는 방식과 메우 비슷하다고 볼 수 있습니다.

## Example sites
---

해당 정보를 얻어낸 사이트입니다. 방문하시면 제가 적지 않는 몇가지 추가적인 팁도 얻어내실 수 있습니다. 

참고로 첫번째 링크에서 몇가지 빠진 정보들이 있는데 웹소켓이나 웹워커같은 기능을 이용하는 방법은 실질적으로 많은 브라우저에서 적용하지 않고 표준도 세워주지 않은 상황이라 아직 적용하기에는 이르다고 생각하여 적지는 않았습니다만 상황에 따라 꼭 기능이 필요한 상황이라면 한 번 써보는 것도 추가적인 성능 향상을 기대할 수 있을 것 같습니다.

[BEST PRACTICES FOR A FASTER WEB APP WITH HTML5](http://www.html5rocks.com/en/tutorials/speed/quick/)

[Fast WebView Applications](http://maxogden.com/fast-webview-applications)