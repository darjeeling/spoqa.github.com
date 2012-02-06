---
layout: entry
title: Eclipse Memory Analyzer 소개
author: akaz
author-email: akaz@spoqa.com
description: Eclipse Memory Analyzer(MAT)을 소개하고 유용하게 쓸 수 있는 기능들을 알아봅니다.
---

안드로이드 개발을 하다보면 종종 [OutOfMemory(OOM)][OOM]에러를 만나게 됩니다. [이전에 올렸던 포스팅](http://spoqa.github.com/2012/01/09/using-gson-in-android.html)에서도 이 문제로 고생을 했는데요, 메모리 누수 관련 문제는 로직 에러와는 달라서 찾기가 매우 난감한 경우가 많습니다. 이러한 메모리 누수 관련 문제를 해결하기 위한 검사 기능을 제공하는 무료 툴이 있습니다. 바로 [Eclipse MAT(Memory Analyzer)(MAT)][MAT]입니다.

![Eclipse MAT](/images/eclipse-mat/mat_thumb.png)

## Eclipse MAT
---
[MAT]은 사용자로 하여금 힙 메모리의 상황을 파악하게 해주어 메모리 누수 현상과 필요없는 메모리 할당을 감지할 수 있도록 도와줍니다. 또한 자동으로 보고서를 작성하여 어떤 객체들이 메모리 누수를 일으키는지에 대한 추측을 해주는 기능을 제공합니다. [MAT]은 [Eclipse] 플러그인이기 때문에 사용하려면 Eclipse가 깔려 있어야 합니다. [MAT]을 설치하려면 [MAT 다운로드 페이지](http://eclipse.org/mat/downloads.php)에서 자신의 Eclipse버전에 맞는 파일을 받으시면 됩니다. 

## How to use MAT
---
MAT을 설치하였다면 Eclipse화면에서 MAT관련 탭이 뜹니다. 탭을 클릭 하고 
     
    File -> Open Heap Dump

를 누르면 힙 상황이 기록 된 [hprof]파일을 읽어올 수 있습니다.

![MAT tab](/images/eclipse-mat/mat_tab.png)
![Open Heap Dump](/images/eclipse-mat/open_heapdump.png)

탭이 뜨지 않는다면 

    Window -> Open Perspective -> Other에서 Memory Analysis

를 누르면 탭이 뜨는 것을 볼 수 있습니다.

![perspective](/images/eclipse-mat/perspective_memory_analysis.png)

[hprof] 파일을 읽어오면 분석을 시작하고 결과를 Overview 화면에 보여줍니다. 

![Overview screen](/images/eclipse-mat/overview.png)

파이 차트의 각 부분에 마우스를 갖다 대면 옆의 Inspector 화면에 해당 객체의 정보를 보여주는 것을 볼 수 있습니다.

### Inspector
---
Inspector 창에서는 선택된 객체의 내용을 볼 수 있습니다. 해당 객체의 클래스명과 패키지 명 그리고 해당 객체가 가지고 있었던 변수의 내용을 살펴볼 수 있습니다.

### 유용한 기능들
---
[MAT]에서 가장 중요하게 살펴볼 기능이라고 한다면 Leak suspoect report와 Dominator tree라고 볼 수 있습니다. Leak suspect와 Dominator tree 둘 다 가장 메모리를 많이 차지하고 있는 객체에 대한 정보를 제공합니다.

![Useful feature](/images/eclipse-mat/overview_useful_method.png)

#### Leak suspect
---

![Leak suspect](/images/eclipse-mat/leak_suspect.png)

Leak suspect는 가장 큰 용량을 차지하고 있는 객체들을 좀 더 세분된 파이 도표로 보여줍니다. Problem suspect 1을 보면 현재 이 스레드 객체의 크기가 전체 힙 메모리의 크기 중 19.73%를 점유하고 있다는 것을 알 수 있습니다. 전체의 20% 가까이 차지하고 있다는 것은 이 객체를 [OOM]의 범인(?)이라고 생각할 근거가 됩니다. 해당 객체에 대한 더 자세한 정보를 얻고 싶다면 Details을 클릭하면 됩니다.

#### Dominator tree
---
Dominator tree를 띄우면 현재 덤프 된 매모리 스냅 샷 중 가장 큰 용량을 차지하고 있는 객체 순으로 정렬하여 보여줍니다. Leak suspect와 비슷해 보이지만 더 구체적인 정보를 제공한다는 점이 다릅니다. 따라서 Leak suspect로 현 상황에 대한 힌트를 얻은 후 Dominator tree에서 디테일하게 살펴보는 것이 시간을 절약하는 방법입니다. 

![Dominator tree](/images/eclipse-mat/dominator_tree.png)

상위에 있는 몇몇 객체들이 가장 의심 되는 객체들이라고 볼 수 있겠습니다. 왼쪽의 화살표를 클릭함으로써 그 객체가 참조하고 있는 다른 객체들에 대한 정보들을 볼 수 있습니다. 각 객체를 클릭하면 옆에 Inspect창의 내용이 달라지는 것을 볼 수 있습니다.

실제 이 스냅 샷은 [이전 포스팅의 문제](http://spoqa.github.com/2012/01/09/using-gson-in-android.html)를 해결하려고 떠놓은 스냅 샷인데요, 이 결과를 보고 많은 메모리가 네트워크를 통해 받아오는 스트림을 처리하고 문자열로 가공하는데에 낭비되고 있다는 생각이 들어 다른 방법으로 우회하는 방법을 썼고 결과적으로 문제를 해결 할 수 있었습니다.

## Android에서 MAT사용법
---
먼저 안드로이드 기기에서 힙 덤프를 수행하여 [hprof]파일을 생성해야 합니다. [hprof]파일을 생성하기 위해서 간단하게 취할 수 있는 2가지 방법이 있습니다.

### 1. DDMS를 이용한 추출
---
Eclipse의 [DDMS]를 이용하여 힙 덤프를 추출할 수 있습니다. 아 방법을 쓰려면 앱의 메니페스트 파일에 [WRITE_EXTERNAL_STORAGE 권한](http://developer.android.com/reference/android/Manifest.permission.html#WRITE_EXTERNAL_STORAGE)을 부여해야 하며, sdcard에 쓸 수 있는 권한이 있어야 합니다. 이 방법을 통해 sdcard경로에 앱 패키지명의 [hprof]파일이 생성됩니다.

![DDMS Heap Dump](/images/eclipse-mat/ddms_heapdump.png)

### 2. Heap dump method
---
안드로이드 API에서 제공하는 메서드 중에 [hprof]파일을 생성하는 메서드인 [dumpHprofData](http://developer.android.com/reference/android/os/Debug.html#dumpHprofData(java.lang.String\))가 있습니다. 이 메서드는 [Debug] 클래스의 메서드인 것을 알 수 있는데, 이 [Debug] 클래스에는 앱의 상태를 점검할 수 있는 여러 유용한 메서드가 있으므로 나중에 필요하면 사용할 수 있도록 익혀두면 좋습니다.

### Android hporf 파일 변환
---
앞서 설명한 방법을 적용하여 [hprof]파일을 추출하였어도 안드로이드에서 추출한 [hprof]파일은 [MAT]에서 받아들이는 일반적인 [hprof]포맷과 다르기 때문에 먼저 변환하는 과정이 필요합니다. 이러한 기능을 제공하는 것이 기본 SDK에 포함된 hprof-conv유틸입니다. 이 유틸은 SDK폴더 내의 tools폴더 안에 있는데 사용하려면 콘솔에서

    $ hprof-conv <안드로이드용 hprof 파일> <변환할 hprof 파일>

를 치시면 됩니다. 이제 변환된 파일을 MAT에서 열면 분석을 하실 수 있습니다.

## More tip
---

[Eclipse Memory Analyser (MAT) - Tutorial](http://www.vogella.de/articles/EclipseMemoryAnalyser/article.html)

[Memory Analyzer Blog](http://memoryanalyzer.blogspot.com/2010/01/heap-dump-analysis-with-memory-analyzer.html)

[Java Performance blog](http://kohlerm.blogspot.com/search/label/memory)

상기의 사이트들은 [MAT]과 Java의 메모리 처리에 관련된 내용을 포스팅한 사이트들입니다. 한 번 들러보면 좋은 정보를 얻을 수 있을것입니다.

[Eclipse]: http://www.eclipse.org/
[MAT]: http://eclipse.org/mat/
[OOM]: http://developer.android.com/reference/java/lang/OutOfMemoryError.html 
[Debug]: http://developer.android.com/reference/android/os/Debug.html
[DDMS]: http://developer.android.com/guide/developing/debugging/ddms.html
[hprof]: http://java.sun.com/developer/technicalArticles/Programming/HPROF.html
