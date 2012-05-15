---
layout: entry
title: A/B Testing에 대한 기초적인 정보들
author: JC Kim
author-email: shinvee@spoqa.com
description: 최근 널리 쓰이고 있는 A/B Testing를 스터디하고 싶으신 분들을 위해 저희들이 보아온 여러가지 자료를 공유하고자 합니다.
---

최근 서비스 개발에서 사용성을 2순위 이하로 두고 있는 개발 그룹은 거의 보기 힘든 것 같습니다. 이런 흐름에 맞추어 여러 가지 사용성 테스트 기법이 관심을 받고 있는데, 역시 가장 적용하기 쉽고 잘 알려져있는 것은 A/B Testing인 것 같습니다. 

스포카 팀도 역시 다양한 요소에 A/B Testing를 진행하고 있는데요. 오늘은 A/B Testing을 공부하고 싶으신 분들을 위해 저희가 보아온 여러 가지 자료를 공유하고자 합니다.

A/B Testing의 정의, 효과
---
![A/B Testing Image by Optimizely](http://www.optimizely.com/static/img/index/features/whatisabtesting.png)

A/B Testing은 전체 디자인에서 한가지 요소에 대한 두 가지 이상의 버전을 시험하여 더 나은 것을 판별하는 기법입니다. 보통은 기존의 버전(A)과 새로운 버전(B)를 가지고 랜덤하게 방문하는 사용자 별로 다른 버전을 보여준 후, 의도하는 결과가 높게 나오는 쪽이 어느 쪽인지를 검증해나갑니다.

A/B Testing의 효과에 대해선 사용성 연구의 대가인 제이콥 닐슨의 두 가지 글를 추천해드립니다.
 
 - [A/B Testing, Usability Engineering, Radical Innovation: What Pays Best?](http://www.useit.com/alertbox/innovation.html)
 - [Putting A/B Testing in Its Place](http://www.useit.com/alertbox/20050815.html)

A/B Testing이 다른 사용성 개선 기법들에 가지는 장단점을 상세히 소개하고 있습니다. 요약하자면, 비용이 적게 들고 빠르게, 빈번하게 수행하면서 리스크가 거의 없이 더 나은 디자인을 획득할 수 있지만 반대로 그 혁신의 폭이 매우 좁은 편입니다. 

즉, 심층 인터뷰 같은 방법으로 얻을 수 있는 근본적인 원인은 찾기 어려우며, 사실 더 나은 것을 고른다 할지라도 어떤 이유에서 그것이 더 많이 선택되었는지는 규명하기 어렵기도 합니다. 쉽게 말하면, 애초에 엉망인 서비스는 A/B Testing 정도로 극적인 변화를 이끌어내기는 어려울 수 있다는 것입니다. :-(

성공적인 A/B Testing 사례
---
A/B Testing이 어떻게 진행이 되고 개선 효과를 보게 될까요? 아래의 글은 루비 온 레일즈 프레임웍과 Getting Reals, Rework 등의 저작으로 유명한 [37signals]의 블로그에 작성된 글입니다. 자사의 [Highrise]라는 서비스의 첫 화면을 어떻게 개선해나갔는지를 자세히 기록하고 있습니다.

![37signal A/B Testing](http://s3.amazonaws.com/37assets/svn/706-summary.png)

 - [Behind the scenes: Highrise marketing site A/B testing part 1](http://37signals.com/svn/posts/2977-behind-the-scenes-highrise-marketing-site-ab-testing-part-1)
 - [Behind the scenes: A/B testing part 2: How we test](http://37signals.com/svn/posts/2983-behind-the-scenes-ab-testing-part-2-how-we-test)
 - [Behind the scenes: A/B testing part 3: Finalé](http://37signals.com/svn/posts/2991-behind-the-scenes-ab-testing-part-3-final)

그들은 이번 A/B Testing을 통해, [Highrise]의 첫페이지에 웃고 있는 사람의 큼직한 사진이 전환률을 높여주며, 어떤 유형의 사람인지는 크게 중요하지 않다는 결론을 도출해내었습니다. 어느 정도 유의미한 수준의 개선을 이루어낸 이후에도, 지속적인 A/B Testing을 통해 유의미한 것과 유의미하지 않은 것을 끝까지 뽑아낸 점이 인상깊습니다.

더 정교한 A/B Testing을 위해
---
A/B Testing을 진행하면서 단순히 더 높은 수치를 기록하는 방안을 바로 선택하는 것도 방법이지만, 좀 더 일관되고 정교한 A/B Testing을 하기 위해선 일반적인 통계기법을 적용해서 유의성을 함께 검증하는 것을 추천해 드립니다. 유의성을 검증한다는 것은, 쉽게 말해 **이게 우연한 일치가 아니라 실제로 차이가 벌어졌다고 볼 수 있는 건지 확인해보는 것입니다.** A/B Testing에 적합한 통계 기법은 t-검정에 대해 조사해보시는 것으로 기반 지식 없이 쉽게 구현해보실 수 있습니다.

 - [Student's t-test](http://en.wikipedia.org/wiki/Student's_t-test)

위의 37signals는 위의 사례 글에 이어서, 통계적인 유의 수준, 검정력을 토대로 한 적정 샘플 크기를 잡는 방법을 깔끔하게 정리해놓았으니 함께 읽어보시는 것을 추천해 드립니다.

 - [A/B Testing Tech Note: determining sample size](http://37signals.com/svn/posts/3004-ab-testing-tech-note-determining-sample-size)

A/B Testing에 도움이 되는 도구들
---
위의 t-검정은 많은 분이 잘 모르시지만 사실 엑셀로도 쉽게 해볼 수 있습니다. 무료로 데이터 분석 기능을 제공하고 있거든요. A/B Test에 대한 원시 자료를 구하여 엑셀에 올려놓을 수 있다면 아래의 데이터 분석 튜토리얼을 참조하여 유의성 검증을 해보시기 바랍니다.

 - [Paired t-test Using Microsoft Excel](http://www.stattutorials.com/EXCEL/EXCEL_TTEST2.html) 

웹서비스를 운영하면서 아주 간편하게 A/B Testing을 수행하고 싶으시다면 Optimizely라는 서비스를 추천합니다. 웹사이트에 Javascript 임베드 코드를 한 줄 삽입하는 것만으로 GUI를 통한 A/B Testing 설계와 수행을 할 수 있습니다.

 - <http://www.optimizely.com/>

   [Highrise]: http://highrisehq.com/
   [37signals]: http://37signals.com/
