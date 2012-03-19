---
layout: entry
title: Eclipse 디버거 사용법
author: akaz
author-email: akaz@spoqa.com
description: Eclipse의 디버거의 사용 방법을 소개합니다.
---

꽤 많은 분들이 디버거의 존재 자체를 모르고 있거나 혹은 디버거가 있다는 사실은 알아도 그 효용성에 의문을 제기하곤 합니다. 왜냐하면, 우리에겐 <code>Log</code> 클래스나 혹은 <code>printf</code>같은 훌륭한(?) 디버깅 도구가 있다고 생각하기 때문이죠. 물론 이렇게 필요한 변수를 찍어보면서 어떤 곳에서 버그가 있는지를 알아보는 일이 잘못된 일은 아닙니다만 복잡한 여러 상황이 맞물려 재현되는 버그는 이러한 고전적인(?) 방법을 써서 알아보기가 매우 어렵습니다.

원인을 정확히 그리고 빨리 파악하려면 디버거의 사용법을 숙지하고 사용하는 것이 가장 좋습니다. 대부분의 개발 환경에서 디버거를 제공하는데 다행히 [이클립스]에서도 쓸만한 디버거를 내장하고 있습니다.

오늘 포스팅에서는 이클립스 디버거 사용법에 대해 다루어 볼까 합니다. 

## 이클립스 디버거 뷰
---
이클립스는 디버거 뷰를 제공하여 디버거를 사용할 수 있도록 합니다. 디버거 뷰는 어디에서 확인할 수 있을까요? 바로 우측 상단에 Debug 뷰에 들어가면 그곳에서 확인할 수 있습니다.

![debugger-view](/images/eclipse-debugger/debugger-view.png)

## 디버깅의 시작
---
그렇다면 어떻게 디버깅을 활성화한 상태로 프로그램을 실행할 수 있을까요? 상단 메뉴의 Run에서 프로그램을 실행할 때 Debug를 이용하여 프로그램을 실행하면 디버거가 작동하게 됩니다.

![run-debug](/images/eclipse-debugger/run-debug.png)

## 브레이크 포인트 설정과 뷰
---
보통 디버깅을 할 때 가장 먼저 하는 일이 브레이크 포인트를 잡는 일입니다. 브레이크 포인트를 에러가 일어나는 라인이나 혹은 의심이 가는 변수를 추적할 수 있는 라인쯤에 잡아놓고 프로그램을 디버깅하면 해당 라인을 실행할 때 디버거가 작동하게 되고 그곳에서 프로그램을 라인 별로 진행해가며 관찰을 진행할 수 있게 됩니다.

브레이크 포인트 설정은 매우 간단합니다. 편집기 왼쪽에 파란 부분(마커 바)을 더블 클릭하게 되면 파란 원이 생기는데 이 원이 브레이크 포인트입니다. 혹은 오른 클릭하여 <code>Toggle break point</code>를 누르면 됩니다. 설정 후 다시 더블 클릭하게 되면 브레이크 포인트가 사라지게 됩니다.

![toggle-breakpoint](/images/eclipse-debugger/toggle-breakpoint.png)

또한, 디버그의 브레이크 포인트 뷰에서 지금까지 걸어놓은 모든 브레이크 포인트들의 위치를 확인할 수 있고 활성화/비활성화, 삭제도 할 수 있습니다. 여러 브레이크 포인트가 걸려있을 때에는 이 탭에서 확인하고 관리하는 것이 더 편합니다.

![breakpoint-view](/images/eclipse-debugger/breakpoint-view.png)

또한, 디버깅을 진행하고 있는 도중에도 다른 의심이 가는 라인에 브레이크 포인트를 걸 수 있습니다.

## 스텝 단위 진행
---
지정한 브레이크 포인트에 다다르면 동시에 디버거가 작동하게 되고 그 라인부터 스텝 단위의 진행을 할 수 있게 됩니다.

![debug-ui](/images/eclipse-debugger/debug-ui.png)

이제 이 뷰의 버튼들을 이용하여 현재 상황을 진행하거나 되돌릴 수 있습니다. 자주 사용하는 버튼의 사용법을 알아보면

1. Resume : 다음 브레이크 포인트를 만날때까지 진행합니다.
2. Suspend : 현재 작동하고 있는 쓰레드를 멈춥니다.
3. Terminate : 프로그램을 종료합니다.
4. Step Into : 메서드가 존재할 경우 그 안으로 들어가 메서드 진행 상황을 볼 수 있도록 합니다.
5. Step Over : 다음 라인으로 이동합니다. 메서드가 있어도 그냥 무시하고 다음 라인으로 이동합니다.
6. Step Return : 현 메서드에서 바로 리턴합니다.
7. Drop to Frame : 메서드를 처음부터 다시 실행합니다.

등이 있습니다. 

실제로 디버깅 화면에서 버튼들을 눌러보면 쉽게 그 쓰임새를 아실 수 있습니다.

## 변수의 상태 확인을 쉽게 해주는 변수 뷰
---
디버깅을 진행하는 도중 변수의 값이나 객체의 상태를 알고 싶은 상황이 생기게 됩니다. 현재 의심이 가는 변수 이외에도 이 변수에 영향을 끼칠 다른 변수들이나 객체들의 상황을 실시간으로 검사할 필요가 있을 때 변수 뷰를 이용하면 도움을 얻을 수 있습니다.

![variables-view](/images/eclipse-debugger/variables-view.png)

이곳에서 변수나 객체의 상태를 확인하고 변수의 상황에 대해서 저장할 수 있습니다. 변수나 객체의 상황을 모두 저장해서 클립보드에 붙이고 싶은 일이 생기면 해당 변수를 오른클릭 후 Copy Variables를 선택합니다.

![copy-var](/images/eclipse-debugger/copy-var.png)

편집 창으로 돌아가 변수에서 <code>Command + shift + i</code>를 누르게 되면(혹은 오른 클릭 후 Inspect를 선택) Inspector 창이 뜨게 됩니다. 이 창에서 다시 한번 <code>Command + shift + i</code>를 누르면 해당 변수를 Expression 뷰로 보내게 되고 이곳에서 지속해서 변수의 상태를 관찰할 수 있게 됩니다.

![inspector](/images/eclipse-debugger/inspector.png)

## Expression 뷰 이용
---
Expression 뷰에서는 변수 이름을 입력하거나 수행해보고 싶은 명령어를 직접 입력하여 그 결과 값을 관찰할 수 있습니다. 결과 값을 관찰할 뿐만 아니라 Expression에 써놓은 변수들은 명시적으로 지우지 않는 이상 계속해서 관찰을 수행하기 때문에 변해가는 상황을 지속해서 관찰할 일이 있는 변수나 명령문을 등록해놓기에 좋습니다.

![expression-view](/images/eclipse-debugger/expression-view.png)

## Display 뷰 이용
---
디스플레이 뷰에서는 현 문맥에서 사용할 수 있는 명령어를 실행하거나 변수의 값을 조작하는 일을 수행하기에 적합한 환경을 제공합니다. Expression에서도 비슷한 기능을 제공하지만, 디스플레이 뷰를 이용하는 것이 더 편합니다. 메모장과 같이 쉽게 쓰고 지울 수 있기 때문입니다.
 
또한, 원본 코드의 수정 없이 편하게 현재의 맥락을 변화시킬 수 있는 것이 가장 큰 장점이라고 볼 수 있습니다.

필요한 명령어들을 적어놓은 후 실행하고 싶은 부분만 드래그하여 수행하거나 혹은 값을 리턴받을 수 있습니다. 지금은 boolean변수 하나의 값을 바꿔보기도 하고 조건 값에 따라 무언가를 리턴 받도록도 해놓은 상황을 스크린 샷으로 담아보았습니다.

값을 반환받고 싶을 때는 두 번째 버튼을, 단순히 실행만 할 때에는 세 번째 버튼을 누르면 됩니다.

![display-get-result](/images/eclipse-debugger/display-get-result.png)
두 번째 버튼을 눌러 값을 반환받는 상황입니다.

![display-execute](/images/eclipse-debugger/display-execute.png)
단순히 실행만 하려면 세 번째 버튼을 누릅니다.

## 브레이크 포인트에 조건 걸기
---
브레이크 포인트에 조건을 거는 것이 굉장히 유용할 때가 있습니다. 특히 반복문안에 들어가 있는 코드들을 디버깅할 때 유용하지요. 반복문의 경우 모든 상황을 검사한다기보다는 특정 조건에서 값이 어떻게 들어가는지를 분석하는 경우가 더 많은데 이러한 상황을 검사하기 위해서 브레이크 포인트에 조건을 걸어야 합니다.

브레이크 포인트를 거는 과정까지는 똑같습니다. 브레이크 포인트를 건 후 그 포인트에서 오른 클릭을 하면 Breakpoint properties 옵션이 있는 것을 확인할 수 있습니다. 이 옵션에서 조건문을 설정하여 디버거의 활성화 조건을 설정할 수 있습니다.

![breakpoint-properties](/images/eclipse-debugger/breakpoint-properties.png)

먼저 <code>Conditional</code>을 활성화하여 어떤 조건에서 디버깅 화면으로 전환할지를 쓰면 되는데 이 창에 조건식을 쓰면 됩니다.

![breakpoint-condition](/images/eclipse-debugger/breakpoint-condition.png)

또 <code>hit count</code>를 이용하여 조건을 걸 수도 있습니다. <code>hit count</code>에 값을 적용하면 해당 라인에 브레이크 포인트가 <code>hit count</code>만큼 잡힌 이후 디버깅 화면으로 전환하게 됩니다. <code>hit count</code> 옵션은 반복문에서 한 100번쯤 이후에 디버깅을 시작하고 싶거나 하는 일이 생길 때 유용하게 쓸 수 있습니다.

![breakpoint-hitcount](/images/eclipse-debugger/breakpoint-hitcount.png)

[Eclipse]: http://www.eclipse.org/
[이클립스]: http://www.eclipse.org/
[MAT]: http://eclipse.org/mat/