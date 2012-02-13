---
layout: entry
title: eventlet을 활용한 비동기 I/O 프로그래밍
author: 문성원
author-email: longfin@spoqa.com
description: 비동기 통신을 지원하는 라이브러리인 eventlet과 그 적용 사례를 소개합니다.
---

안녕하세요. [스포카] 크리에이터팀 문성원입니다. 현대적인 프로그래밍 환경에서 네트워크는 더는 특정 직군의 개발자만 접하는 분야가 아닙니다. 그런 만큼 대량의 요청을 네트워크를 통해 송수신하는 프로그램이 생각보다 성능이 나오지 않는 경우를 경험하신 분들도 많으실 겁니다. 물론 스포카 개발팀도 예외는 아니었습니다. 그래서 오늘은 저희의 이러한 경험과 그 해결책-[eventlet]을 통한 [비동기 I/O\(Asynchronous I/O\)][Asynchronous I/O]-에 대해 소개합니다.

# Why
---

우선 스포카 개발팀에서 겪었던 문제부터 시작하죠. 얼마 전 [페이스북(facebook)][facebook]의 [FQL(Facebook Query Language)][FQL]를 통해 정보를 수집해서 이를 활용하는 기능을 작성해야 했습니다. 기존의 함수들은 필요할 때마다 [FQL]을 요청하는 방식이었고 당연히 이건 너무 느렸죠. 그래서 생각한 것이 "하루의 일정 시간마다 대량의 [FQL] 요청을 보내서 필요한 정보를 미리 갱신시켜놓자."였습니다. 여기까진 좋았죠. 이때 제가 작성한 코드의 얼개를 살펴보면 대강 이렇습니다.

<script src="https://gist.github.com/1808721.js"> </script>

그런데 문제가 있었습니다. 기존의 [FQL]을 보내는 <code>FacebookAccount.update()</code>는 [FQL]요청이 완료될때까지 멈추고 기다립니다. 대부분의 [FQL]요청이 2, 3초 정도 걸린다고 했을 때 이러한 지연은 매우 치명적입니다. 대안이 필요했고 자연스레 떠오른 것이 서두에 소개한 [비동기 I/O\(Asynchronous I/O\)][Asynchronous I/O]였습니다.

## Asynchronous
---

과거 일부 고급 서버 개발자만 알고 있는(혹은 알아야 하는) 기술로 치부되던 '비동기(Asynchronous)'란 개념은 2000년대 들어 등장한 [Ajax(Asynchronous JavaScript and XML)][Ajax]의 성공 이후 많은 개발자에게 강한 인상을 줬습니다. 사용자는 HTTP 요청이 끝날 때까지 멈추어 있는 하얀 화면으로부터 해방되었고, 다양하고 많은 요청과 응답들이 자연스럽게 서버로 흘러들어 가서 나왔습니다. 개발자들의 이러한 경험과 통찰은 이후 [node.js]와 같은 플랫폼의 등장에도 많은 영향을 끼쳤습니다.

다시 문제로 돌아가죠. 그렇다면 이러한 비동기에 관한 개념은 위의 상황을 어떻게 해결할 수 있을까요? 문제의 원인부터 다시 살펴봅시다. 2, 3초 정도씩 걸리는 [FQL] 요청이 문제일까요? 물론 요청이 매우 빨리 처리된다면 별도의 처리 없이도 저 코드는 문제없이 동작합니다. 하지만 현실적으로 이런 I/O의 속도를 빠르게 하는데에는 물리적으로 한계가 있습니다. 오히려 여기에서 주목해야 할 점은 '2, 3초' 보다 '기다린다'라는 점입니다. <code>FacebookAccount.update()</code> 같은 경우, I/O가 처리되는 동안 CPU는 하던 일을 멈추고 문자 그대로 기다리게 됩니다. 만약 CPU가 멈추지 않고 다른 요청을 보낸다면 어떨까요? 이렇게 말이죠.

![blocking vs non-blocking](/images/concurrency-and-eventlet/blocking_vs_non_blocking.png)


## 비동기만으로는 부족하다?
---

이러한 아이디어는 그동안 많은 개발자가 대량의 I/O를 다루는 올바른 방식으로 여겨왔습니다. 하지만 보통 이러한 비동기 I/O를 통한 구현은 동기식 I/O와는 좀 다른 형태를 띠게 됩니다. 이렇게 말이죠.

<script src="https://gist.github.com/1808715.js"> </script>

불행하게도, 이 경우 기존에 사용하던 <code>urllib2</code>대신 HTTP 요청을 처리하는 핸들러를 이처럼 재작성 해야합니다. 거기에 <code>FacebookAccount.update()</code>의 호출 방식마저 바뀔 수 있죠. 더군다나 콜백(Callback) 투성이의 코드는 유지보수가 쉬어 보이지도 않습니다. 여러모로 손이 많이 가는 상황이죠.

결국, 기존 코드를 최대한 수정하지 않으면서도, 어느 정도 성능은 보장되는 그런 해결책이 필요했습니다. 그런 해결책이 있을까요? 다행히도 그렇습니다.

# What
---

저희가 해결책으로 택한 [eventlet]은 [Python]\(정확히는 [CPython]\)에서 [코루틴(Coroutine)][코루틴]을 지원하기 위해 만들어진  [greenlet]을 이용해 작성된 네트워크 관련 라이브러리입니다. 생소한 용어가 갑자기 튀어나와서 놀라셨을지도 모르니 우선 [eventlet]에 대해 설명하기 전에 앞에 나온 용어들을 찬찬히 한번 살펴보죠.

## [코루틴]과 [greenlet]
---

먼저 [코루틴(Coroutine)][코루틴]부터 살펴보죠. 전산학도라면 누구나 그 이름을 한번은 들어봤을 [도널드 카누쓰(Donald Knuth)](http://ko.wikipedia.org/wiki/%EB%8F%84%EB%84%90%EB%93%9C_%ED%81%AC%EB%88%84%EC%8A%A4)는 자신의 저서 [The Art of Computer Programming](http://ko.wikipedia.org/wiki/The_Art_of_Computer_Programming)에서 [코루틴]을 다음과 같이 설명합니다.

> Subroutines are special cases of more general program components, called “coroutines.” In contrast to the unsymmetric relationship between a main routine and a subroutine, there is complete symmetry between coroutines, which call on each other.

[코루틴]은 우리가 잘 알고 있는 [서브루틴(Subroutine)][서브루틴]과 달리 진입점(Entry Point)이 여러 개일 수 있습니다. 쉽게 이야기하면 실행을 멈췄다가(Suspend) 재개(Resume)할 수 있다는 점인데요. 이 특성을 살리면 우리가 익히 아는 [스레드(Thread)][스레드]처럼 쓸 수 있게 됩니다. 다만 [스레드]와 달리 [코루틴]은 [비선점적(Non-Preemptive)](http://en.wikipedia.org/wiki/Nonpreemptive_multitasking)이기때문에 코드의 흐름을 전적으로 사용자가 제어할 수 있습니다.

![coroutines](/images/concurrency-and-eventlet/coroutines.png)

하지만 불행히도 모든 언어에서 이런 [코루틴]이 지원되진 않습니다. [greenlet]은 이런 [코루틴]을 [CPython]에서 지원하기 위해 작성된 라이브러리입니다. 

## [eventlet]
---
[코루틴]을 통해 [스레드]를 대체할 수 있다는 점에 주목한 사람들은 [greenlet]을 통해 유용한 네트워크 라이브러리를 만들어냈습니다. [eventlet]도 그 중 하나죠. 잠시 [eventlet]의 소갯글을 봅시다.

> Eventlet is a concurrent networking library for Python that allows you to change how you run your code, not how you write it.

위에서 볼 수 있듯이 [eventlet]은 사용성에 중점을 두었습니다. 기존의 블로킹 I/O 스타일의 프로그래밍에 익숙한 개발자들도 쉽게 비동기 I/O의 장점을 얻을 수 있게끔 하는 게 목적이죠. 

특히 저희가 주목한 점은 [eventlet]의 [멍키패치 기능](http://eventlet.net/doc/patching.html#monkey-patch)입니다. 멍키패치는 본래 [동적 언어에서 런타임에 코드를 고쳐서 별도의 파일 변경 없이 본래 소스의 기능을 변경하는 것을 말합니다](http://en.wikipedia.org/wiki/Monkey_patch). [eventlet]은 [<code>eventlet.monkey_patch</code>] 메서드를 통해 표준 라이브러리의 I/O 라이브러리를 논블러킹으로 동작하게끔 변경해서 [코루틴]에 적합하게 만듭니다.

# How
---

앞서 소개한 [<code>eventlet.monkey_patch</code>]를 이용하면 실제로 고칠 부분은 정말로 적어집니다. 다음 코드가 [eventlet]을 이용해 변경한 전부입니다.

<script src="https://gist.github.com/1808724.js"> </script>

정말 적죠? 조금만 구체적으로 살펴보죠. 우선 [<code>eventlet.monkey_patch</code>]는 <code>socket</code>이나 <code>select</code>등의 [Python] 표준 라이브러리를 <code>eventlet.green</code> 패키지안에 정의된 [코루틴] 친화적인 모듈들로 바꿔치기 합니다. 

<script src="https://gist.github.com/1812372.js"> </script>

이렇게 바꿔치기된 <code>eventlet.green</code>안의 모듈들은 I/O에 의해 블럭되는 경우 다른 코루틴에 제어권을 넘기는 식으로 지연을 방지합니다.

![with_coroutines](/images/concurrency-and-eventlet/with_coroutines.png)


# 다른 대안들
---

사실 이러한 목적으로 사용되는 라이브러리는 [eventlet]만 있는 것은 아닙니다. [gevent]는 [eventlet]에서 영향을 받았지만, [libevent]를 기반으로 하여 더욱 나은 성능과 성숙한 인터페이스를 갖추고 있습니다. 저희처럼 [libevent]의 설치에 제한이 있는 환경이 아니라면 이쪽을 살펴보셔도 좋습니다.

만약 [이벤트 주도적 프로그래밍(Event-Driven Programming)](http://en.wikipedia.org/wiki/Event-driven_programming)에 흥미가 있으신 분은 [Twisted] 역시 좋은 대안이 될 수 있습니다. 


[스포카]: http://spoqa.com
[eventlet]: http://eventlet.net/
[Asynchronous I/O]: http://en.wikipedia.org/wiki/Asynchronous_I/O
[facebook]: http://www.facebook.com/
[FQL]: http://developers.facebook.com/docs/reference/fql/
[node.js]: http://nodejs.org/
[Ajax]: http://en.wikipedia.org/wiki/Ajax_(programming)
[eventlet]: http://eventlet.net/
[Python]: http://python.org/
[CPython]: http://wiki.python.org/moin/CPython
[코루틴]: http://en.wikipedia.org/wiki/Coroutine
[서브루틴]: http://en.wikipedia.org/wiki/Subroutine
[greenlet]: http://pypi.python.org/pypi/greenlet
[스레드]: http://en.wikipedia.org/wiki/Thread_(computing)
[<code>eventlet.monkey_patch</code>]: http://eventlet.net/doc/basic_usage.html?highlight=monkey_patch#eventlet.monkey_patch
[gevent]: http://www.gevent.org/
[libevent]: http://libevent.org/
[Twisted]: http://twistedmatrix.com/trac/
