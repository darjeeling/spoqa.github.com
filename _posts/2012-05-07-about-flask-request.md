---
layout: entry
title: Werkzeug와 Flask의 Request
author: 문성원
author-email: longfin@spoqa.com
description: Werkzeug와 Flask가 HTTP 요청을 어떻게 추상화 하는지를 살펴봅니다.
---

오늘은 [이전 포스트](http://spoqa.github.com/2012/01/16/wsgi-and-flask.html)에서 소개해 드린 [WSGI] 유틸리티인 [Werkzeug]와 이를 사용하는 프레임워크인 [Flask]가 [HTTP] 요청을 어떻게 추상화하는지를 구체적으로 살펴보도록 하겠습니다. 

이번 포스트에 첨부된 코드는 Github 저장소([Flask 0.8](https://github.com/mitsuhiko/flask/tree/0.8), [Werkzeug 0.8.3](https://github.com/mitsuhiko/werkzeug/tree/0.8.3))에서 인용하겠습니다. 혹시 좀 더 궁금한 점이 있으시다면, 직접 코드를 내려받아 확인해보시는 것도 좋습니다.


# Down the Rabbit-Hole
---

우선 [HTTP]로 요청받은 쿼리스트링(querystring)을 그대로 출력하는 간단한 애플리케이션부터 시작해봅시다.

<script src="https://gist.github.com/2594329.js?file=simple_app.py"></script>

<code>@app.route()</code>와 같은 코드는 이제 익숙하실 겁니다. 등록된 핸들러가 문자열을 반환하면 이를 응답으로 돌려주는 것 역시 이해하기 크게 어렵지 않습니다. 


그런데 <code>request</code>는 과연 어떨까요? <code>import</code>문을 통해 부르는 것으로 보아 외부 모듈에 정의된 변수 같기는 한데, 매번 쿼리를 바꿀 때마다 그에 맞춰서 자동으로 그 값이 바뀝니다. 특별히 <code>app</code>나 <code>handler</code>로부터 값을 전달받거나 하는 것 같지도 않습니다. 


한번 <code>type()</code>을 통해 확인해볼까요?

<script src="https://gist.github.com/2594329.js?file=what_is_this.py"></script>

불행하게도 브라우저에는 아무것도 출력되지 않습니다. 이번엔 [pdb]를 통해 디버그해 봅시다. (단 [Flask]의 <code>debug</code> 옵션은 <code>False</code>로 놓아야 합니다.)

<script src="https://gist.github.com/2594329.js?file=gistfile1.txt"></script>

<code>dir()</code>을 통해 대강 무엇을 하는 객체인지 짐작을 할 순 있지만, <code>type()</code>으로 확인한 타입은 <code>Request</code>가 아닌 <code>werkzeug.local.LocalProxy</code>라는 알쏭달쏭한 타입입니다. 과연 이 <code>request</code>의 진짜 정체는 뭘까요?

# Back to basics
---

잠깐 여기서 처음으로 돌아가 보죠. [WSGI] 애플리케이션은 [HTTP] 요청을 처리하기 위한 함수(혹은 부를 수 있는 객체)로 <code>environ</code>과 <code>start_response</code>를 컨테이너로부터 받아 요청을 처리합니다. [Flask]는 <code>Flask.wsgi\_app()</code>을 통해 이 부분을 처리합니다.

<script src="https://gist.github.com/2594329.js?file=wsgi_app.py"></script>

여기서 실제로 <code>environ</code>에 담긴 [HTTP] 요청에 대한 상세를 <code>Request</code> 형태로 바꾸는 역할을 하는 것이 바로 <code>request\_context()</code>입니다. <code>request\_context()</code>는 현재 클라이언트로부터 받은 <code>environ</code>을 토대로 문자 그대로 요청(Request)에 대한 문맥(Context)을 만들어서 이를 핸들러에서 사용할 수 있게끔 합니다. 

<script src="https://gist.github.com/2594329.js?file=request_context.py"></script>

여기서 만들어 내는 <code>RequestContext</code>에 대해서 조금만 더 자세히 봅시다.

<script src="https://gist.github.com/2594329.js?file=RequestContext.py"></script>

<code>RequestContext</code>는 넘겨받는 <code>app</code>의 <code>request_class</code>(기본은 뒤에 살펴볼 <code>werkzeug.wrappers.Request</code>입니다.)를 통해 문맥 안에서 사용할 <code>request</code>를 초기화합니다. 또한 [<code>with</code> 구문](http://docs.python.org/reference/compound_stmts.html#the-with-statement)을 통해 실행되는 <code>\_\_enter\_\_()</code>를 통해 자기 자신을 <code>\_request\_ctx\_stack</code>에 넣습니다.

여기서 주목해야 할 것이 <code>\_request\_ctx\_stack</code>입니다. 이 스택은 <code>.global</code>에 정의되어있습니다.  

<script src="https://gist.github.com/2594329.js?file=global.py"></script>

<code>\_request\_ctx</code>는 [Werkzeug]의 <code>LocalStack</code>의 인스턴스입니다. 또 우리가 여태까지 찾아 헤매던 <code>request</code> 역시 여기에 정의되어있는데, 이는 <code>\_request\_ctx</code> 중 가장 위의 <code>.request</code>를 가져오게 되어있는 <code>LocalProxy</code>임을 알 수 있습니다.

# Globalocal
---

그렇다면 과연 <code>LocalStack</code>과 <code>LocalProxy</code>는 무엇이며, 왜 필요한 것일까요? 백문이 불여일견이란 말도 있듯이 코드를 봅시다. 이 두 클래스는 <code>werkzeug</code>에 정의되어 있습니다.

<script src="https://gist.github.com/2594329.js?file=local.py"></script>

코드가 약간 복잡하지만, 기본적으로는 <code>get_ident</code>를 이용해서 데이터를 저장하고 가져옵니다. <code>get\_ident</code>는 현재 문맥([스레드(Thread)][스레드]나 [코루틴(Coroutine)][코루틴])을 나타내는 식별자로, 2개의 클래스는 모두 개개의 문맥에서 전역적으로 사용할 수 있는 값으로 저장될 수 있습니다.([Java]의 [java.lang.ThreadLocal]나 [Clojure]의 [binding](http://clojure.github.com/clojure/clojure.core-api.html#clojure.core/binding)을 떠올리시면 이해가 쉽습니다.) 스레드별로 다른 요청을 처리하는 경우 각각의 요청 문맥이 분리되어야 하기 때문이죠.

이제 정리해봅시다. <code>environ</code>을 통해 넘어온 요청 내용은 <code>request_context()</code>에 의해 <code>Request</code> 객체로 만들어지고 이 객체는 현재 문맥에서 전역적으로 사용할 수 있게끔 <code>\_request\_ctx\_stack</code>에 저장됩니다. 우리는 핸들러에서 이를 <code>Flask.request</code>로 접근하여 사용합니다.

# How it works?
---

이제 <code>request</code> 객체가 어디서 만들어지는지를 살펴보았으니 어떻게 만들어지는지에 초점을 맞춰보죠. <code>request</code> 객체는 아까 잠깐 언급한 <code>request_class</code>, 즉 별다른 설정이 없다면 [<code>werkzeug.wrappers.Request</code>](https://github.com/mitsuhiko/werkzeug/blob/0.8.3/werkzeug/wrappers.py#L1619)를 구현합니다. 

[<code>werk.zeug.wrappers.Request</code>](https://github.com/mitsuhiko/werkzeug/blob/0.8.3/werkzeug/wrappers.py#L1619)는 [<code>werkzeug.wrappers.BaseRequest</code>](https://github.com/mitsuhiko/werkzeug/blob/0.8.3/werkzeug/wrappers.py#L71)에 [HTTP] 요청에 대한 명세를 독립적으로 구현하는 몇 개의 다른 [믹스인(MixIn)][믹스인]으로 구성되어있습니다. 기본적인 구현 전략은 <code>environ</code>을 객체의 멤버 변수로 가지고 있다가 조회하는 메소드나 프로퍼티가(ex. <code>.values</code>, <code>forms</code>)가 처음으로 불릴 때 명세에 맞게 이를 불러옵니다. ([<code>.\_load\_from\_data</code>](https://github.com/mitsuhiko/werkzeug/blob/0.8.3/werkzeug/wrappers.py#L299))

# Prologue
---

지금까지 살펴본 과정은 사실 [Flask]가 [HTTP] 요청을 어떻게 처리하는지에 대한 시작점에 지나지 않습니다. 이러한 <code>Request</code>가 처리되고 적절한 [HTTP] 응답으로 변환되는 과정에도 살펴볼 점이 많은데, 이는 기회가 되면 다음 시간에 다뤄보도록 하겠습니다.

[WSGI]: http://www.wsgi.org/en/latest/index.html
[Flask]: http://flask.pocoo.org
[Werkzeug]: http://werkzeug.pocoo.org/
[HTTP]: http://ko.wikipedia.org/wiki/HTTP
[pdb]: http://docs.python.org/library/pdb.html
[Java]: http://en.wikipedia.org/wiki/Java_(programming_language)
[java.lang.ThreadLocal]: http://docs.oracle.com/javase/6/docs/api/java/lang/ThreadLocal.html
[Clojure]: http://clojure.org
[코루틴]: http://en.wikipedia.org/wiki/Coroutine
[스레드]: http://en.wikipedia.org/wiki/Thread_(computing)
[믹스인]: http://en.wikipedia.org/wiki/Mixin

