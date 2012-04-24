---
layout: entry
title: ASIHTTPRequest를 대체하는 iOS 네트워킹 라이브러리 2가지
author: JC Kim
author-email: shinvee@spoqa.com
description: iOS 앱을 개발하는데 있어 네트워킹 라이브러리의 선택은 중요합니다. 이번 글에선 최근 개발이 종료된 ASIHTTPRequest 라이브러리의 대안을 2가지 소개합니다.
---

![ASIHTTPRequest](http://allseeing-i.com/i/smallerlogo.png)

[ASIHTTPRequest]는 iOS 개발자들 사이에서 가장 많이 이용되는 네트워킹 라이브러리인데, 간결한 인터페이스와 개선된 성능으로 인기를 끌었습니다. Github의 [Objective-C](https://github.com/languages/Objective-C) **Most Watched Overall에서도 2위** 자리를 현재도 유지하고 있는 것을 보면 이 라이브러리가 얼마나 오랜 시간 동안 iOS 개발자들에게 사랑받았는지는 쉽게 알 수 있습니다.

[request release];
---
하지만 애석하게도, 이 라이브러리는 작년 9월에 제작 종료가 선언되었습니다. 6개월 이상 된 소식이지만 하도 오랜 시간 동안 쓰여와서 소개된 곳이 많다보니 제작 종료 소식이 많이 안 퍼지고 있는 듯합니다. 

여러 가지 이유가 있겠지만, 제작자는 [제작 종료 선언 글](http://allseeing-i.com/[request_release];)을 통해 **"내부가 너무 복잡해졌고, 수 년에 걸쳐 누적된 몇 가지 아키텍처 선택이 프로젝트를 유지 보수하기 어렵게 만들었다."**라고 제작 종료 선언의 이유에 대해 고백하고 있습니다. 

부지런히 갈아탈 준비를 해두세요.
---
제작 종료가 선언된 라이브러리인 만큼 가능하면 새로운 라이브러리로 갈아타시는 것이 좋습니다. iOS 개발환경은 1년 단위로 빠르게 성장하고 있는데, 당장 최근 iOS5 개발환경만 해도 [block] 문법 기반의 API 패러다임, [ARC] 지원들이 현행 라이브러리들의 필수 요소처럼 굳어져 가고 있습니다. 이에 맞추어 따라갈 수 있는 라이브러리들을 쓰는 것이 장기적인 개발 환경 개선에 도움이 될 것입니다. 

어떤 대안이 있나?
---
[ASIHTTPRequest] 라이브러리 개발자는 여러 가지 대안을 소개했지만, 저는 2가지 정도로 간추려서 추천하고자 합니다. 하나는 [AFNetworking]이며, 하나는 [MKNetworkKit]입니다.

AFNetworking
---
![AFNetworking](https://github.com/AFNetworking/AFNetworking/raw/gh-pages/afnetworking-logo.png)

[AFNetworking]은 최근 Facebook에 인수된 [Gowalla](http://gowalla.com/)에서  NSURLConnection, NSOperation 등의 기본 Foundation framework 위에 구현된 네트워킹 라이브러리입니다. 

현재 ASIHTTPRequest의 대안으로 가장 빠르게 성장하고 있는 라이브러리인데, 그 이유는 유명 애플리케이션 개발사의 개발자들이 유지하고 있는 프로젝트이면서, 꽤 명쾌한 API를 제공하고 있습니다. 기본적인 [block] 기반의 API 구성 외로도, [SDWebImage]와 같은 라이브러리에서 볼 수 있는 이미지 다운로드 헬퍼도 제공하고 있어 매우 편리합니다.

자세한 사용법은 [AFNetworking] Github 저장소에서 확인할 수 있습니다.

MKNetworkKit
---
[ASIHTTPRequest]는 편리한 API를 제공해주는 것으로 많은 사용자에게 사랑받았지만, 기본 NSURLConnection, NSOperation 으로 낼 수 없는 높은 퍼포먼스 또한 그의 강점이었습니다. [MKNetworkKit]은, ASIHTTPRequest의 아키텍처와 AFNetworking의 인터페이스를 동시에 지향하고자 하는 라이브러리입니다. 그 외에도 아래와 같은 기능들을 추가로 겸비합니다.

 - 전체 앱에 대한 single queue 관리
 - 자동 queue 크기 조절
 - 캐싱과 복구 기능
 - 비슷한 request를 하나의 처리로 수행
 - Full [ARC] support

아주 멋진 목표를 가지고 진행되고 있는 프로젝트이며 개발 진척도 상당히 빠른 속도로 진행 중이지만, 아직 자잘한 버그가 많다는 것이 단점입니다. 네트워킹 라이브러리는 애플리케이션 단위에선 상당히 저 수준에 있는 만큼, 이 문제는 치명적일 수 있습니다. 그래서 상업용 프로젝트에 바로 이용하기보다는 실험적인 프로젝트에서 써보면서 지켜보는 것을 추천합니다.

마무리하며
---
iOS 애플리케이션 개발 환경에서 네트워킹 라이브러리의 선택은 개발 속도와 애플리케이션 퍼포먼스에서 아주 중요한 위치에 속합니다. [ASIHTTPRequest]는 그 중 가장 많이 쓰였지만, 개발 종료를 선언했기 때문에 대안 라이브러리를 준비하시는 것이 좋습니다. 

[AFNetworking]은 편리하게 쓸 수 있는 API를 NSURLConnection, NSOperation 위에 구현하였으며, 믿을 수 있을 만큼 성숙하여 현재 새 프로젝트에 바로 도입하기 좋습니다. [MKNetworkKit]은 아직 개발이 한창 더 진행되어야 하지만 API 디자인과 개선된 퍼포먼스, ARC 지원 등 보다 미래지향적인 목표를 하고 있으므로 장기적으로 지켜볼 가치가 있습니다.

이 외에도 추천하는 라이브러리가 있다면 공유해봅시다.

  [block]: http://developer.apple.com/library/ios/#documentation/Cocoa/Conceptual/Blocks/Articles/00_Introduction.html
  [ARC]: http://clang.llvm.org/docs/AutomaticReferenceCounting.html
  [ASIHTTPRequest]: http://allseeing-i.com/ASIHTTPRequest/
  [AFNetworking]: https://github.com/AFNetworking/AFNetworking
  [MKNetworkKit]: https://github.com/MugunthKumar/MKNetworkKit
  [SDWebImage]: https://github.com/rs/SDWebImage
