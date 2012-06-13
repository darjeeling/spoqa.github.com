---
layout: entry
title: 패키지 이름을 통해 디폴트 브라우저 띄워보기
author: akaz
author-email: akaz@spoqa.com
description: 최근 앱 개발중 있었던 문제를 살펴보면서 패키지명을 통해 인텐트를 띄우는 방법을 알아봅니다.
---

최근 앱을 개발하면서 인텐트를 통해 URL을 전달하여 외부 브라우저를 띄울 일이 있었습니다. 문제가 있다면 저는 디바이스에 설치된 기본 브라우저를 이용하고 싶었는데 안드로이드는 비슷한 의도를 가진 인텐트를 처리할 여러 앱을 보여준다는 것이었습니다. 가령 URL을 처리할 여러 브라우저를 보여주는 식이지요. 따라서 가장 오래전에 설치된 브라우저를 띄울 방법을 찾아야 했습니다.

### 앱의 패키지명 얻어오기
---

그래서 일단 저는 디바이스에 깔린 브라우저들의 패키지 명을 전부 알아낼 수 있는 방법을 조사해보았습니다.

<script src="https://gist.github.com/2724011.js?file=1.java"></script>

먼저 실제로 호출할 법한 인텐트를 만드는 것을 볼 수 있습니다. 카테고리를 설정하고 [Uri](http://developer.android.com/reference/android/net/Uri.html) 정보를 제공함으로써 이 인텐트는 브라우저를 호출할 인텐트라는 것을 알 수 있습니다. 그리고 만들어진 인텐트를 앞서 얻어온 패키지 매니저의 [queryIntentActivities매서드](http://developer.android.com/reference/android/content/pm/PackageManager.html#queryIntentActivities\(android.content.Intent, int\))에 인자로 넣어주게 되면 ResolveInfo타입의 리스트를 얻어낼 수 있습니다. [ResolveInfo타입](http://developer.android.com/reference/android/content/pm/ResolveInfo.html)은 인텐트 필터를 이용해 분석한 인텐트 정보를 담는 클래스인데, 이 리스트를 순회하면서 현제 디바이스에 깔린 브라우저들의 패키지 명을 얻어낼 수 있는 것을 볼 수 있습니다. 실제 앱 관련 패키지에 관한 정보는 activityInfo.applicationInfo로 접근하셔서 얻어낼 수 있습니다.

### APK 생성 시점 알아보기
---

모든 패키지 명을 알아냈다고 하더라도 이 중에서 무엇을 호출할지가 또 문제가 되죠. 앱을 설치한 시점, 즉 마지막으로 갱신된 시점을 가지고 기본으로 제공된 브라우저를 알아낼 수 있다는 가정하에 설치된 앱의 갱신 시점을 알아보는 방법을 사용하게 되었습니다.

<script src="https://gist.github.com/2724011.js?file=2.java"></script>

[ApplicationInfo클래스](http://developer.android.com/reference/android/content/pm/ApplicationInfo.html)에서 sourceDir멤버변수를 통해 전체 파일 경로를 얻어낼 수 있고 이를 통해 [File클래스](http://developer.android.com/reference/java/io/File.html)의 lastModified메서드를 이용하여 앱의 마지막 갱신 시점을 알아낼 수 있습니다.

### MAP을 통해 정렬
---

이제 정렬을 하여 가장 옛날에 갱신된 브라우저를 호출하면 됩니다. 저의 경우 정렬을 하기 위해 Map을 사용하여 키값을 정렬하는 방식을 이용했지만, 다른 방식으로 정렬해도 상관은 없습니다.

<script src="https://gist.github.com/2724011.js?file=3.java"></script>

마지막으로 패키지 매니저의 [getLaunchIntentForPackage메서드](http://developer.android.com/reference/android/content/pm/PackageManager.html#getLaunchIntentForPackage\(java.lang.String\))에 앱의 패키지 명을 넣으면 다른 사용 가능한 앱을 무시하고 해당 패키지 명을 가진 앱을 호출하게 됩니다. 따라서 디폴트 브라우저를 통한 브라우징이 가능합니다.

### 마치며
---

조금 특수한 상황에서 인텐트 호출법에 대해 알아보았습니다. 비슷한 상황을 처리해야 할 분들에게 지금 보여 드린 코드 스니펫이 조금이라도 도움이 되었으면 좋겠습니다.