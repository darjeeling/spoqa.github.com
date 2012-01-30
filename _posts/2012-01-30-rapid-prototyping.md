---
layout: entry
title: 빠른 프로토타이핑을 위한 도구 소개
author: JC Kim
author-email: shinvee@spoqa.com
description: 스포카 팀은 매장에서부터 페이스북, 트위터까지 이어지는 총체적인 경험 선을 시험하기 위해 프로토타이핑에 많은 고민을 하였습니다. 이번 글에선 프로토타이핑을 빠르게 할 수 있게 도와주는 도구들을 소개합니다. 
---


새로운 아이디어를 검증하는 방법은 여러 가지가 있습니다. 비슷한 도구들을 사용하면서 간접체험을 해보기도 하고, 종이, 혹은 목업 도구를 활용한 프로토타입을 만들어보기도 합니다.

스포카 팀은 새로운 아이디어를 검증하기 위해 더욱 직접적인 프로토타입을 만들어야 했습니다. 매장에서의 오프라인 경험에서부터 Facebook, Twitter 등의 온라인 경험까지 이어지는 총체적인 경험 선을 시험하기 위해선 실제로 어느 정도 동작하는 프로토타입을 만드는 것이 제일 확실하였기 때문이죠.

하지만 막상 그럴싸하게 동작하는 프로토타입을 만드는 것은 생각보다 시간이 오래 걸리는 일입니다. 최소의 UI 디자인, 빠른 기능 개발, 배포 환경이 제대로 준비되어있지 않다면 실제로 유효한 수준까지 만드는 것이 많은 시간이 필요하게 될 것입니다.

스포카 팀은 아이디어가 떠오를 때 어떻게 하면 그것을 빠르게 구현해서 확인할 수 있을지에 대해 많이 고민하였습니다. 그러면서도 해당 아이디어가 좋을 때 제대로 된 서비스로 확장하거나 기존 기능에 통합하는 것도 수월하게 가능하다면 더 좋겠지요. 이번 글에서는 제대로 작동하면서 확장성도 고려한 고 수준의 프로토타이핑을 빠르게 할 수 있게 도와주는 도구들을 모두 소개해보고자 합니다.

## 어떤 언어를 고를까?
---

특별히 교육의 목적이 있는 것이 아니라면 언어는 자신이 가장 잘 활용할 수 있게 미리 교육된 언어가 효과적입니다. 새로운 언어를 공부하면서 프로토타이핑을 한다면 지엽적이고 모르는 문제에 부딪혀 시간을 허비하는 상황이 많아 프로토타이핑 속도가 지연되기 쉽기 때문입니다. 다만 컴파일 가능한 언어와 불가능한 언어 중 선택해야 한다면 대부분 컴파일 과정이 필요없는 언어를 선택하는 것이 큰 효과를 경험하실 수 있습니다.

스포카 팀은 서버 개발에 [Python]을 주 언어로 활용하며, 그 외에 [Ruby]나 [Node.js] 같은 언어도 추천합니다.

## 마이크로 프레임워크를 활용하자
---
규모가 커지면 구조에 손을 대야 하지만, 다양한 기능을 빨리 구현해서 넣고자 할 때 마이크로 프레임워크로 시작하는 것이 좋습니다. 간편하면서도 초기 구조를 아주 간결하게 들고 갈 수 있기 때문입니다.

웹 서비스나 앱 서비스의 서버로 이용할 HTTP 프로토콜 서버를 구축한다면 Sinatra 스타일의 마이크로 프레임워크를 활용하는 것이 효과적입니다. 

<script src="https://gist.github.com/1694116.js?file=gistfile1.txt"> </script>

아래는 주요 언어에서 볼 수 있는 마이크로 프레임워크입니다. 이 외에도 [Sinatra style microframework](http://bit.ly/ynbOfn)을 검색해보시면 여러 언어에서 비슷한 형태로 구현된 마이크로 프레임워크를 보실 수 있습니다.

 - [Sinatra](http://www.sinatrarb.com/) ([Ruby])
 - [Flask](http://flask.pocoo.org/) ([Python])
 - [Express](http://expressjs.com/) ([Node.js])

스포카팀에서는 [Flask]를 즐겨쓰고 있습니다. Flask에 관심이 있으시다면 [지난 기술 블로그의 소개글](http://spoqa.github.com/2012/01/16/wsgi-and-flask.html)을 참조해주세요.

## 디자인을 빠르게 하는 툴킷들
---
기본적인 기능들을 빠르게 구현하였다면 이를 활용할 사용자 인터페이스를 만들어야 합니다. 하지만 웹 서비스나 웹뷰를 기반으로 하는 서비스를 만든다면 HTML/CSS/JS 기반의 디자인을 하는 일도 상당히 시간이 많이 필요한 일입니다. 이 때, 각 목적에 맞는 툴킷들을 이용한다면 디자인을 크게 고민하지 않으면서도 보기 좋은 서비스를 만들어 볼 수 있습니다.

[Bootstrap from Twitter]는 디자인에 대한 여러 가지 기초적인 고민을 상당히 잘 흡수해주는 훌륭한 툴킷입니다. 크로스 브라우징을 지원하며, 우리가 쓰는 컴포넌트 대부분에 대해 심미적으로, 기능적으로 우수한 디자인을 제공합니다. 그리드 인터페이스를 제공해서 레이아웃도 간편하게 잡을 수 있으며, 곧 출시 예정인 2.0에선 [반응형 디자인]도 정식으로 지원하고 있습니다. 

Bootstrap은 [LESS]로도 제공해주기 때문에, 디자인 튜닝이 간편하고 [Mixin](http://lesscss.org/#-mixins)을 활용해 의미적인 HTML 마크업을 하면서 디자인을 적용할 수도 있습니다.

위의 툴킷과 같은 인터페이스를 가지고 디자인만 [Facebook] 형태로 바꾼 [Fbootstrapp]도 있습니다. Facebook 앱을 만든다면 이쪽을 쓰시는 편이 더 좋을 것 같습니다.

터치 환경에 한정한 서비스를 디자인 중이라면 범용성이 조금 떨어지지만 [jQuery Mobile]을 추천합니다. 여러 기기의 웹뷰 환경을 지원하는 다양한 컴포넌트를 제공하고 있습니다.

## 서비스를 최대한 쓰기
---
모든 기능을 직접 전부 구현할 필요는 없습니다. 여러 회사에서 한 두 줄의 추가만으로 사용할 수 있는 서비스를 제공하고 있습니다. [Google]은 특히 [Maps API](http://code.google.com/intl/ko/apis/maps/index.html), [Chart Tools](http://code.google.com/intl/ko/apis/chart/), [QR Code][Google Infographics], [Font API](https://developers.google.com/webfonts/) 등 개발에 도움이 되는 수많은 기능들을 간단한 API로 쓸 수 있게끔 공개하고 있으며, [Facebook] 또한 [소셜 플러그인](https://developers.facebook.com/docs/plugins/)으로 다양한 소셜 도구들([Like Button](https://developers.facebook.com/docs/reference/plugins/like), [Comments](https://developers.facebook.com/docs/reference/plugins/comments), [Registration](https://developers.facebook.com/docs/plugins/registration/) 등)을 제공하고 있습니다. 이런 서비스들을 잘 알고 있다면 가끔은 단지 여러 서비스 기능을 연결하는 것만으로 새로운 서비스를 만들 수 있기도 합니다.


## 서비스 배포는 Platform as a Service(PaaS)를 활용하자
---
위 도구의 협력으로 서비스를 만들었다면 이제 배포를 해야 합니다. 어디서나 접근할 수 있는 공용 서버에 서비스를 올리고, 서버를 세팅하고, 도메인을 연결해야 합니다. 이 과정들 또한 시간을 많이 필요로 하는 일들입니다.

최근 [Heroku]를 시작으로 미국에서 [Amazon Web Service]를 기반으로 한 많은 [Platform as a Service][PaaS]가 출시되고 있습니다. 이 서비스들은 대체로 **[Failover System], 쉬운 서비스 규모 스케일링, 잘 설계된 서버 스택, 편리한 배포환경**을 강점으로 내세우고 있으며, 특히 처음 사용자가 가입부터 서비스 배포까지 아주 간편하고 빠른 속도로 진행할 수 있게끔 도구를 제공하고 있습니다. 게다가, 대부분 무료 플랜이 존재하기 때문에 비용 부담이 없다는 장점도 가지고 있습니다.

[Heroku]의 서비스 배포 과정을 보시면 그 과정이 얼마나 편리한지 쉽게 알 수 있습니다.

<script src="https://gist.github.com/1694125.js?file=gistfile1.sh"> </script>

단 두 줄로 git에 의해 관리되는 애플리케이션을 서버에 배포하고 접근 URL을 받았습니다.

아래는 다양한 플랫폼에서 쉽게 이용 가능한 [PaaS] 목록입니다.

  - [Heroku]: [Ruby], [Java], [PHP], [Python], [Node.js] 기반의 서비스 배포 플랫폼 제공.
  - [Dotcloud]: 가장 많은 종류의 서비스 스택 지원.
  - [phpfog]: [PHP]에 특화된 서비스.
  - [ep.io]: [Python]에 특화된 서비스. 불안정하지만 쉽게 이용 가능.

## 저장소 이용
---
아무리 빠르게 하고 싶다고 해도 저장소는 두고 하세요. 개인이 작업하는 것이라면 로컬에서도 저장소 관리가 가능한 [분산형 버전관리 시스템][DVCS] \([git], [mercurial]\)로 바로 이용하시고, 2명 이상이 동시에 작업한다면 반드시 저장소 호스팅 서비스를 이용해서 작업하시기 바랍니다. 변경사항을 공유하는 방법에 대해 버전관리 시스템보다 빠르고 깔끔한 방법은 아직까진 없기 때문입니다.

저장소 호스팅은 많은 곳에서 제공해주고 있지만, 돈을 조금 투자해서 [Github]를 쓰시는 것을 추천해 드립니다. 저장소뿐만이 아닌 훌륭한 협업 플랫폼을 제공해주고 있기 때문입니다. 당장은 무료로 시작해야 한다면 [Bitbucket]의 무료 비공개 저장소를 이용하는 것도 좋은 방법니다.

## 실제 케이스
---
아래는 최근 사내에서 이루어진 아이디어 서비스 프로토타이핑이 이루어진 과정을 나열해보았습니다.

 1. [Github]에 저장소 생성. 팀원들에게 전달
 2. 한 명은 [Flask]로 서버 사이드 개발
 3. QR코드 생성이 필요한 부분을 [Google API][Google Infographics]로 해결
 4. 한 명은 [Bootstrap from Twitter]로 뷰 작업을 진행
 5. 작업이 되는대로 [Github], [Heroku]에 배포

개발에 필요한 시간은 **약 5시간 정도**였으며, 사실 이 기간은 그 이전에 **해당 아이디어의 가치에 대해 토론하는 데 쓴 시간**과 비슷한 시간이었습니다. 토론에선 답이 나오지 않은 채로 끝났지만, 프로토타입을 이용해보고 답을 내는 것은 그리 오랜 시간이 걸리지 않았습니다.

## 마치며
---
실용 가능한 프로토타이핑은 앱의 첫인상과 인터페이스 전반에 대한 이해를 넘어 아이디어의 가치평가를 확신할 수 있는 좋은 방법입니다. 우리가 토론에서 의견이 많이 갈리는 이유는 사실 보지 못한 것에 관해 이야기하기 때문인 경우가 많아서, 만약 토론하는 시간보다 더 짧은 시간 안에 말하는 것을 볼 수 있다면 의사 결정을 더 빠르고 정확하게 할 수 있습니다. 이 글은 그 방법에 대해 구체적으로 설명하였습니다.

이번에 소개한 도구와 방법은 단지 돌아가는 것을 확인하는 것을 넘어 장기적인 확장성도 갖추고 있습니다. 언급한 언어들 모두 대형 서비스에서 실제 이용 중인 언어들이며, 마이크로 프레임워크들도 모두 커지는 구조에 대한 대응법을 준비하고 있습니다. 디자인은 Bootstrap의 일부 코드를 재작성하거나 튜닝하는 것으로 서비스에 최적화시킬 수 있으며, [PaaS]는 애초에 Fast scaling이 주요 강점이기 때문에 손쉽게 커지는 서비스의 사용량에 유연하게 대처할 수 있습니다.

새로운 아이디어를 준비하고 계신다면, 이 글에서 소개한 도구들을 십분 활용하여 빠르게 실용할 수 있고, 확장 가능한 프로토타입을 반복해서 만들어 보시는 것을 적극 추천해 드립니다.

  [Python]: http://python.org/
  [Ruby]: http://www.ruby-lang.org/
  [Java]: http://www.java.com/
  [PHP]: http://php.net/
  [Node.js]: http://nodejs.org/
  [Flask]: http://flask.pocoo.org/
  [LESS]: http://lesscss.org/
  [Google]: http://google.com/
  [Facebook]: http://facebook.com/
  [Bootstrap from Twitter]: http://twitter.github.com/bootstrap/
  [반응형 디자인]: http://www.alistapart.com/articles/responsive-web-design/
  [Fbootstrapp]: http://ckrack.github.com/fbootstrapp/
  [jQuery Mobile]: http://jquerymobile.com/
  [Failover System]: http://en.wikipedia.org/wiki/Failover
  [Amazon Web Service]: http://aws.amazon.com/
  [PaaS]: http://en.wikipedia.org/wiki/PaaS
  [Heroku]: http://heroku.com/
  [Dotcloud]: http://dotcloud.com/
  [phpfog]: http://phpfog.com/
  [ep.io]: http://ep.io/
  [DVCS]: http://en.wikipedia.org/wiki/Distributed_revision_control
  [git]: http://git-scm.com/
  [mercurial]: http://mercurial.selenic.com/
  [Github]: http://github.com/
  [Bitbucket]: http://bitbucket.org/
  [Google Infographics]: http://code.google.com/intl/ko/apis/chart/infographics/
