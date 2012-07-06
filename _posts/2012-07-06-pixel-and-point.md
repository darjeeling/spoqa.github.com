---
layout: entry
title: 픽셀과 포인트
author: 최재형
author-email: monday@spoqa.com
description: 픽셀과 포인트, 그리고 DPI에 대해서 소개합니다.
---

안녕하세요? 스포카에서 디자인 총괄을 맡고있는 최재형입니다. 오늘은 픽셀<sub>pixel</sub>과 포인트<sub>point</sub>에 대해 알아보겠습니다.

Pixel
---
픽셀은 디스플레이에서 가장 기본이 되는 단위입니다. 디스플레이는 수많은 픽셀들이 모여 구성됩니다. 하나의 픽셀은 일반적으로 R<sub>빨강</sub>, G<sub>초록</sub>, B<sub>파랑</sub> 세 가지의 서브 픽셀<sub>subpixel</sub>을 가지고 있습니다. 우리가 px이라고 부르는 단위는 디스플레이에서 서브픽셀이 모여 색상을 표현하는 최소 단위라 보시면 됩니다. 픽셀에 대한 자세한 이야기는 제 이전 글을 참고해 보시길 바랍니다.

Point
---
포인트<sub>point</sub>는 타이포그래피에서 사용하는 가장 작은 인쇄 단위로, 파이카<sub>pica</sub>를 잘게 나눈 것입니다. 즉, 포인트와 파이카 모두 글자의 크기를 측정하기 위한 단위입니다. 참고로 파이카는 18세기에 만들어진 타이포그래피의 유닛입니다. 1 pica가 정의하는 길이는 다양하지만, 현재 가장 보편적으로 사용되는 방식은 1 pica가 1/6in라고 보시면 됩니다. 이를 미터법으로 표시하면 약 4.233 mm가 됩니다. 간단히 정리하자면 다음과 같습니다.

<center>1 pica = 1/6 inch = 4.233 mm</center>
<br>
그리고 포인트는 파이카를 12조각으로 나눈 단위입니다. 즉 1 pica는 12 pt와 같습니다. 결국 정리하면 1 pt는 다음과 같습니다.

<center>1 inch = 6 pica
<br>
1 pica = 12 pt
<br>
1 pt = 1/12 pica = 1/72 inch = 약 0.3527 mm
</center>

1 pt는 몇 px일까요?
---
1pt는 몇 px일까요? 디자인을 하시는 분이 거나, 디자인을 코드로 구현하는 분이라면 한 번쯤 품어봤을 궁굼증일 겁니다. 

포인트<sub>point</sub>와 파이카<sub>pica</sub>는 컴퓨터가 없을 때부터 있었던 개념입니다. 이 단위가 컴퓨터 디스플레이안에 들어오기 시작하면서 문제가 발생하기 시작합니다. 컴퓨터 디스플레이의 기본이 되는 픽셀의 크기가 모두 다르기 때문입니다.

글자의 크기는 디스플레이의 해상도와 실제 모니터의 사이즈에 영향을 받습니다. 픽셀의 크기는 스크린의 종류마다 제각각이기에 픽셀의 크기와 물리적 단위인 인치<sub>inch</sub> 사이에는 고정된 관계가 없습니다. 그래서 컴퓨터 상에서 글자의 크기는 논리적 단위<sub>logical unit</sub>를 통해 파악하게 됩니다. 

72 pt의 글씨는 1 logical inch 크기의 글씨라고 정의됩니다. 그리고 이 logical inch는 실제 표시되는 물리적 단위<sub>physical unit</sub>인 physical inch로 변환되어 최종적으로는 픽셀로 스크린 위에 표시됩니다.

하지만 논리적 단위가 물리적 단위로 변환되는 기준은 경우에 따라 다릅니다. 이 부분에 대해 알아보기 위해 여기서 잠시 그래픽 유저 인터페이스 역사로 들어가보겠습니다.

WYSIWYG
---
1970년대, Xerox PARC 연구소에서는 WYSIWYG을 개발합니다. WYSIWYG은 "What You See Is What You Get"의 줄임말 입니다. 우리말로는 "보는 대로 얻는다" 라는 뜻의 내용입니다. 

WYSIWYG은 컴퓨터 스크린 상에서 보이는 편집 상태의 화면이 최종 결과물 - 그 당시에는 출력물을 뜻합니다 - 과 같은 방식의 GUI 기반의 편집 툴을 일컫습니다. 우리가 현재 쓰고있는 워드프로세서나 Adobe의 Photoshop, Illustrator 등과 같은 그래픽 편집 툴이 WYSIWYG이라고 보시면 됩니다.


72 dpi
---
Xerox PARC 연구소는 1 point가 1/72 inch인 것에 착안하여 72 ppi<sub>pixels-per-inch</sub>의 화면해상도를 가진 디스플레이를 표준으로 채택합니다. 72 ppi 해상도에서는 1 pt가 1 px이 되게 됩니다. 

Apple은 72 ppi의 규격을 채용하여 OS에 적용합니다. 타자기 세대에 널리 쓰이던 10pt 크기의 글씨를 모니터 상에서 10 px로 보이게 하겠다는 의도였습니다. 

참고로 ppi<sub>pixels-per-inch</sub>와 dpi<sub>dots-per-inch</sub>는 서로 다른 개념이지만 최근에는 차이의 구분 없이 혼용되어 쓰이고 있습니다.

Apple 72 dpi를 채택한 이유는 타자기 세대에서 널리 쓰이던 10 point 크기의 서체를 컴퓨터 스크린에서도 같은 숫자의 크기인 10 px의 크기를 갖게 하겠다는 의도였습니다. 결과적으로 Apple의 선택은 대중에 큰 호응을 얻었습니다. 그리고 Apple PC의 대중화와 함께 72 dpi는 보편화가 됩니다.

96 dpi
---
반면 Microsoft는 96 dpi를 표준 해상도로 채택하게 됩니다. Microsoft가 Apple과는 달리 96 dpi를 표준 해상도로 설정한 이유는 다음과 같습니다.

Microsoft는 인간이 컴퓨터 디스플레이를 볼 때는 책이나 문서를 볼 때 눈(망막)에서 책이나 문서까지의 거리보다 1/3 가량 더 멀리 본다고 주장합니다. 그래서 72 dpi 스크린에서의 글씨는 실제 편집자에 의해 의도되었던 글씨 크기보다 작게 보이기 때문에 가독성에 해를 끼친다고 판단합니다.

![list](/images/2012-07-06/1.png =700x)

<sub><a href="http://cdn.iphonehacks.com/wp-content/uploads/2012/03/apple-new-ipad-retina-display-math.jpg">어디선가</a> 본 것 같은 논리입니다.</sub></center>

Microsoft는 위 문제를 해결하기 위해 트릭을 씁니다. 72 dpi 해상도를 96 dpi의 해상도로 높이는데 그 이유는 다음과 같습니다. 컴퓨터 스크린은 책이나 문서를 읽을 때의 거리 보다 4/3배 이므로, 해상도도 4/3배를 하여 72 ppi의 모니터에서 글씨 크기를 키워 상대적으로 같은 글씨 크기를 만들어 가독성을 높이겠다는 의도였습니다.

Microsoft의 운영체제에서는 위의 96 dpi를 논리적인 해상도<sub>logical resolution</sub>라 부르고, 실제 디스플레이가 지원하고 실질적으로 표현되는 72 dpi의 해상도를 실제 해상도<sub>physical resolution</sub>이라 말합니다.

1 pt = ?
---
다시 궁금즘의 시작점으로 돌아와 1 pt는 몇 px인지에 대해 결론을 내보겠습니다. 1 inch를 표시하기 위해 Mac에서는 72px이 필요하고, Windows에서는 96 px이 필요한 셈입니다. 결국 1 pt가 정의하는 픽셀은 해상도의 상황에 따라 달라지게 됩니다. 

<center>
Mac: 1 inch = 72 px
<br>
Windows: 1 inch = 96px
<br>
<br>
Mac (72 ppi): 1 pt = 1 px = 약 0.3527 mm
<br>
Windows (96 ppi): 1 pt = 1 px * 96/72 = 약 1.3333 px = 약 0.4702 mm
</center>

Web Browser
---
하지만 웹 브라우져 상에서의 해상도는 약간 예외적입니다. (아직까지는) 가장 보편적으로 사용되고 있는 웹 브라우저인 Microsoft의 Internet Explorer는 역시 자사제품답게 96 dpi의 해상도를 가지고 있습니다. 하지만 Google Chrome이나 Apple의 Safari가 쓰는 Webkit 엔진의 경우도 72 dpi가 아닌 96 dpi의 해상도를 기준으로 삼고 있습니다. 이는 CSS가 계획될 당시 96 dpi를 기준으로 만들어졌기 때문입니다. 그러므로 웹에서의 폰트는 포인트에서 4/3배가 된 픽셀 값을 가지게 됩니다. 12 pt의 크기의 글씨는 웹 브라우져에서 16 px로 렌더링 되는 것입니다.

Display
---

![list](/images/2012-07-06/2.png =700x)

<a href="http://members.ping.de/~sven/dpi.html">DPI Calculator / PPI Calculator</a>

하지만 모든 모니터의 도트피치는 다 다릅니다. 위의 링크에서 볼 수 있듯이 모니터의 ppi는 종류별로 매우 상이하죠. 위에서 예로 든 12 pt의 글씨는 웹 브라우져에서 16 px로 렌더링 되긴 하지만, 각 모니터에서 표시되는 16 px의 실제 크기는 모니터의 도트피치에 따라 다릅니다. 

예를들어 1920*1200의 해상도를 가진 24 inch의 모니터는 94.34 PPI<sub>pixel-per-inch</sub>입니다. 그렇다면 위 모니터에서의 16 px의 크기는 다음과 같습니다.

<center>16 px = 16/94.34 inch = 0.1696 inch</center>

하지만 제 맥북프로는 1280*800 해상도에 13 inch의 디스플레이를 탑재하고 있으므로 116.11 PPI<sub>pixel-per-inch</sub>입니다. 그러므로 16 px의 크기는 실질적으로 다음과 같이 보입니다.

<center>16 px = 16/116.11 inch = 0.1378 inch</center>

Reference
---
1.	<a href="http://msdn.microsoft.com/en-us/library/windows/desktop/ff684173(v=vs.85).aspx">http://msdn.microsoft.com/en-us/library/windows/desktop/ff684173(v=vs.85).aspx</a>
2.	<a href="http://blogs.msdn.com/b/fontblog/archive/2005/11/08/490490.aspx">http://blogs.msdn.com/b/fontblog/archive/2005/11/08/490490.aspx</a>
3.	<a href="http://en.wikipedia.org/wiki/Dots_per_inch#DPI_or_PPI_in_digital_image_files">http://en.wikipedia.org/wiki/Dots_per_inch#DPI_or_PPI_in_digital_image_files</a>
