---
layout: entry
title: 안드로이드 오픈 라이브러리
author: akaz
author-email: akaz@spoqa.com
description: 유용하게 쓰일 수 있는 안드로이드 라이브러리를 몇가지 살펴봅니다.
---

안드로이드 개발을 하면서 곧바로 적용할만한 라이브러리들이 무엇이 있을지 항상 궁금했었는데, 이번 기회에 조사를 해 보았습니다. 앞으로 소개할 라이브러리가 조금이라도 프로젝트에 도움이 되었으면 합니다.

#### Actionbar Sherlock
---

구글은 허니콤 이후부터 기존의 타이틀바를 대체한 액션바를 이용하도록 권고하였습니다만 프로요나 진저브레드같은 허니콤 이전의 플랫폼에서는 액션 바를 사용할 수 있는 방법이 없었습니다. 그렇지만 [액션바 셜록](http://actionbarsherlock.com/)을 이용하면 그 이전 버전의 안드로이드에서도 액션바를 사용할 수 있습니다. 또한 ICS스타일의 UI컴포넌트들도 제공하므로 ICS버전 앱과 그 이전 버전 앱의 통일성을 갖추고 싶은 분들은 필히 이용해 봐야 할 라이브러리입니다.

액션 바 셜록의 세팅과 간단한 적용에 대한 [비디오](http://www.youtube.com/watch?v=4GJ6yY1lNNY&feature=player_embedded)를 참조하시면 시작하는데 도움이 될 것 입니다.

액션바에 대해 더 알고 싶은 분들은 [커니님의 블로그 포스팅](http://androidhuman.tistory.com/entry/%EC%95%A1%EC%85%98%EB%B0%94Action-bar-%EB%94%B0%EB%9D%BC%EC%9E%A1%EA%B8%B0-%EC%95%A1%EC%85%98%EB%B0%94%EA%B0%80-%EB%AD%94%EA%B0%80%EC%9A%94)을 확인해보세요. 실제 앱에 적용된 액션바와 UI컴포넌트들을 보고 싶다면 [다운로드 페이지](http://actionbarsherlock.com/download.html)에서 샘플 APK파일을 받을 수 있습니다.

#### RoboGuice, AndroidAnnotations
---

[RoboGuice](http://code.google.com/p/roboguice/)와 [AndroidAnnotations](https://github.com/excilys/androidannotations)는 자바의 [어노테이션](http://docs.oracle.com/javase/tutorial/java/javaOO/annotations.html)을 이용하여 소스를 더 간결하게 짤 수 있도록 도와주어 유지보수를 쉽도록 하고, 코드의 가독성을 높여주는 라이브러리입니다. 또 흔히 일어나는 예외처리등을 손쉽게 다룰 수 있도록 도와주기도 합니다.

비슷한 의도를 가졌지만, 구현 방식은 조금 다른데, [RoboGuice 예제](http://code.google.com/p/roboguice/wiki/SimpleExample?tm=6)와 AndroidAnnotations을 [액티비티에 적용한 예제](https://github.com/excilys/androidannotations/wiki/Enhance%20Activities) 그리고 [Background annotation](https://github.com/excilys/androidannotations/wiki/WorkingWithThreads)을 확인해보시면 이 두 라이브러리가 어떤 의도를 가진 라이브러리인지 알 수 있을 거라고 생각합니다.

#### greendroid
---

[그린드로이드](http://greendroid.cyrilmottier.com/)는 기존 안드로이드에서 제공하는 뷰나 위젯을 더 편리하게 쓸 수 있도록 강화 된 뷰를 제공하는 라이브러리입니다.

가령, 기본적으로 안드로이드에서 제공하는 리스트뷰 같은 경우 리스트의 중간 중간에 헤더를 넣으려면 직접 뷰를 커스터마이징을 해야 했었는데, 그린드로이드에서 제공하는 리스트뷰는 그러한 기능을 기본적으로 제공합니다.

[문서](http://greendroid.cyrilmottier.com/reference/packages.html)도 상당히 잘 정리해 놓은 편인데, 아직까지는 많은 컴포넌트를 지원하지 않지만 계속 발전할 가능성이 높은 라이브러리라고 생각됩니다.

#### ViewPagerIndicator
---

안드로이드 호환성 패키지에 포함된 [ViewPager](http://blog.naver.com/PostView.nhn?blogId=huewu&logNo=110116958816)를 이용하여 화면의 전환 효과를 쉽게 구현할 수 있었습니다. 그런데 ViewPager에 인디케이터를 적용하려면 따로 구현을 해야만 했었는데요, 이러한 인디케이터효과를 쉽게 얻을 수 있도록 만든 라이브러리가 있습니다. [ViewPagerIndicator](http://viewpagerindicator.com/)를 이용하면 쉽게 미리 만들어 놓은 몇가지의 인디케이터들을 적용 할 수 있습니다.

#### NineOldAndroid
---

Honeycomb API에서 지원하는 뷰 에니메이션 관련 API들을 그 이전 API에서도 사용할 수 있게 도와주는 라이브러리입니다. 기존의 API에서 제공했던 에니메이션 API의 지원이 그렇게 풍부하진 못했는데 이 API를 사용함으로써 앱에 더 많은 시각적 효과를 줄 수 있을거라 기대합니다.

구체적인 사용법은 [사용법 페이지](http://nineoldandroids.com/#usage)를 참조하면 될 것 같습니다. import구문만 적용하면 따로 코드를 바꾸지 않아도 바로 적용 가능할 수 있게 잘 만들어져 있습니다.

#### android-query
---

안드로이드에서 빈번히 일어나는 UI조작이나 비동기 작업을 쉽게 할 수 있도록 도와주는 라이브러리입니다. 

간단히 몇 줄만의 코드만으로 네트워크 요청을 통해 JSON, XML, html, 파일등 여러 형식의 데이터를 쉽게 받아올 수 있도록 잘 추상화 해놓은 유용한 [콜백 메서드](http://code.google.com/p/android-query/wiki/AsyncAPI)를 제공합니다.

제가 가장 마음에 드는 기능이 [이미지 로드, 캐싱](http://code.google.com/p/android-query/#Image_Loading)인데 이미지를 쉽게 가져올 수 있도록 도와줄뿐만 아니라 자동으로 특정 폴더에 이미지 캐싱을 해주기 때문에 머리아프게 캐싱 기능을 따로 구현할 필요가 없어보였습니다.

[binding](http://code.google.com/p/android-query/#Binding)도 재미있는 기능인데, 해당 view에 마치 jquery의 bind같이 문자열로 함수 이름을 적어주면 이벤트가 일어났을때 해당 함수가 불리게 됩니다.

그 외에 자잘하게 편의를 도와주는 [여러 API들](http://code.google.com/p/android-query/wiki/API)을 제공합니다.

#### Commonsguy github
---

[Commonsguy](https://github.com/commonsguy)가 만들어 놓은 많은 안드로이드 프로젝트를 모아놓은 저장소입니다. 원래 [The Busy Coder's Guide to Android Development](http://www.amazon.com/Busy-Coders-Guide-Android-Development/dp/0981678009)라는 책에서 교육용으로 사용한 여러 예제를 모아놓은 저장소인데, 라이브러리는 아니지만 프로젝트를 뒤지다 보면 바로 가져다가 쓸 수 있을만큼 완성도 있게 만들어 놓은 컴포넌트들을 많이 발견할 수 있습니다.

[서적의 PDF 파일](http://commonsware.com/Android/Android-1_0-CC.pdf)이 인터넷에 공개되어 있는 것 같은데, 책 내용도 훌륭하므로 한 번 읽어보면 도움이 많이 될 것입니다.


#### android pull to refesh
---

여러 앱에서 사용하는 pull-to-refresh효과를 리스트뷰를 통해 구현한 [라이브러리](https://github.com/chrisbanes/Android-PullToRefresh)입니다.
