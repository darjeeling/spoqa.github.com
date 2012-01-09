---
layout: entry
title: Java의 json 라이브러리 google-gson
author: akaz
author-email: akaz@spoqa.com
description: 안드로이드 주소록을 다루며 애먹었던 경험을 소개하고 간단하게 json라이브러리 google-gson 사용법을 소개합니다.
publish: true
---

## 문제 상황
---

안드로이드 어플리케이션을 개발하다 보면 주소록을 다루는 일이 종종 있습니다. 어플리케이션에서 주소록에 관련된 정보를 접근할 일이 있는 어플이라면 [ContentResolver](http://developer.android.com/reference/android/content/ContentResolver.html)를 통해 단말의 주소록에 접근해서 필요한 정보를 가져오게 됩니다.

그런데, 최근 개발하고 있는 [스포카] 어플을 통해 아주 많은 사람의 연락처가 저장된 주소록을 가지고 이런 저런 로직을 실행하는 상황을 테스트 하다보니, [OutOfMemory(OOM)][OOM]에러가 발생하는 현상을 볼 수 있었습니다. 모바일 디바이스들은 PC와 다르게 자원이 제한적이기 때문에 어떻게 하면 [OOM]을 일으키지 않을 수 있을까 라는 고민을 해야 하는 상황이었습니다.

대강 문제가 되었던 클라이언트 사이드의 로직을 살펴보면 이렇습니다.

1. 단말의 주소록에 접근하여 필요한 정보를 추출 후 서버에 전송
2. 서버에서 정보를 가공하여 필요한 [json] 문자열을 생성 후 반환, 이 문자열은 주소록에서 보낸 정보의 양에 비례해서 늘어나게 됩니다.
3. 클라이언트 측에서 서버 측에서 보낸 [json] 문자열을 이용하여 [JSONObject]객체를 만든 후 이 [JSONObject]를 이용 리스트 완성

[eclipse]의 [MAT(Memory Analyzer)][MAT]을 이용하여 어느 시점에서 [OOM]이 일어나는지를 추측해보았습니다. 서버에서 보내준 [json]형식의 문자열을 [HttpURLConnection]을 통해 전달받고 이를 [StringBuilder]를 이용하여 완전한 문자열으로 만들던 도중에 [OOM]이 일어나는 것으로 의심되었는데 이 때문에 [JSONObject]의 생성자에 [json] 문자열을 전달하기도 전에 메모리가 가득 차 버리니 매우 난감한 상황이었습니다.

대게 주소록에 사람이 그렇게 많지 않으므로 (200~500명 정도) 아무런 문제가 없었지만 10000명 정도의 더미데이터를 주소록에 저장하고 테스트하다 보니 append 메서드를 호출하다 [OOM]에러를 뱉으면서 어플이 종료되었습니다. 문제는 append 메서드를 호출 시 [StringBuilder]의 capacity를 넘을 경우 내부적으로는 메모리 재할당과 copy과정이 일어난다는 것이었습니다. 그렇다고 초기 [StringBuilder]생성시 capacity를 무작정 높게 잡기도 애매한 상황이었습니다.

## gson
---

[gson]은 Java객체를 [json]형식으로 변환하고 그 역으로도 변환할 수 있도록 도와주는 라이브러리입니다. [gson]의 사용법이 궁금하다면 [gson user guide]를 읽어보면 되고 api가 궁금하다면 [gson api document]를 참조하면 됩니다.

## gson 적용
---

<script src="https://gist.github.com/1580601.js?file=gistfile1.py">  </script>

대략 이런 방식으로 프로젝트에 [gson]라이브러리를 적용하였고, [HttpURLConnection]을 통해 받아온 [InputStream]을 이용 바로 객체를 생성할 수 있었습니다. 이전에 [StringBuilder]를 이용할때 생기는 오버헤드가 사라진 셈이죠. 위와 같은 방식으로 [OOM]이 생기는 문제 상황을 해결 할 수 있었습니다.

위의 예는 상황을 최대한 단순화하여 설명하려고 작성한 예제이고 이 [사이트](http://www.softwarepassion.com/android-series-parsing-json-data-with-gson/)를 통해 더 상세하게 설명된 사용예를 보실 수 있습니다.

  [스포카]: http://spo.qa/get
  [OOM]: http://developer.android.com/reference/java/lang/OutOfMemoryError.html 
  [eclipse]: http://www.eclipse.org
  [MAT]: http://eclipse.org/mat/
  [JSONObject]: http://developer.android.com/reference/org/json/JSONObject.html
  [StringBuilder]: http://developer.android.com/reference/java/lang/StringBuilder.html
  [HttpURLConnection]: http://developer.android.com/reference/java/net/HttpURLConnection.html
  [InputStream]: http://developer.android.com/reference/java/io/InputStream.html
  [json]: http://www.json.org
  [gson]: http://code.google.com/p/google-gson/
  [gson user guide]: https://sites.google.com/site/gson/gson-user-guide
  [gson api document]: http://google-gson.googlecode.com/svn/trunk/gson/docs/javadocs/index.html
  [stackoverflow]: http://stackoverflow.com/
