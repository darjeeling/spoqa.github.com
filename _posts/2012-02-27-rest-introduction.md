---
layout: entry
title: REST 아키텍처를 훌륭하게 적용하기 위한 몇 가지 디자인 팁
author: 김재석
author-email: shinvee@spoqa.com
description: 최근의 서버 프로그램은 여러 웹 브라우저는 물론이며, 아이폰, 안드로이드 애플리케이션과의 통신에 대응해야 합니다. 이번 글에선 여러 문제를 지혜롭게 대처할 수 있는 REST 아키텍처에 대해 알아봅니다.
publish: true
---

최근의 서비스/애플리케이션의 개발 흐름은 멀티 플랫폼, 멀티 디바이스 시대로 넘어와 있습니다. 단순히 하나의 브라우저만 지원하면 되었던 이전과는 달리, 최근의 서버 프로그램은 여러 웹 브라우저는 물론이며, 아이폰, 안드로이드 애플리케이션과의 통신에 대응해야 합니다. 그렇기 때문에 매번 서버를 새로 만드는 수고를 들이지 않기 위해선 범용적인 사용성을 보장하는 서버 디자인이 필요합니다. 

REST 아키텍처는 Hypermedia API의 기본을 충실히 이행하여 만들고자 하는 시스템의 디자인 기준을 명확히 확립하고 범용성을 보장하게 해줍니다. 이번 글에선 현대 서비스 디자인을 RESTful하게 설계하는 기초적인 내용에 대해 정리하려고 합니다.

## REST란 무엇인가?

REST는 Representational state transfer의 약자로, 월드와이드웹과 같은 분산 하이퍼미디어 시스템에서 운영되는 소프트웨어 아키텍처스타일입니다. 2000년에 [Roy Fielding](http://en.wikipedia.org/wiki/Roy_Fielding)에 의해 처음 용어가 사용되었는데, 이 분은 HTTP/1.0, 1.1 스펙 작성에 참여했었고 아파치 HTTP 서버 프로젝트의 공동설립자이기도 합니다.

REST는 HTTP/1.1 스펙과 동시에 만들어졌는데, HTTP 프로토콜을 정확히 의도에 맞게 활용하여 디자인하게 유도하고 있기 때문에 디자인 기준이 명확해지며, 의미적인 범용성을 지니므로 중간 계층의 컴포넌트들이 서비스를 최적화하는 데 도움이 됩니다. REST의 기본 원칙을 성실히 지킨 서비스 디자인은 "RESTful 하다." 라고 흔히 표현합니다.

무엇보다 이렇게 잘 디자인된 API는 서비스가 여러 플랫폼을 지원해야 할 때, 혹은 API로서 공개되어야 할 때, 설명을 간결하게 해주며 여러 가지 문제 상황을 지혜롭게 해결하기 때문에 (버전, 포맷/언어 선택과 같은) REST는 최근의 모바일, 웹 서비스 아키텍처로서 아주 중요한 역할을 하고 있습니다.

## 중심 규칙

REST에서 가장 중요하며 기본적인 규칙은 아래 두 가지입니다.

 - URI는 **정보의 자원**을 표현해야 한다.
 - 자원에 대한 행위는 **HTTP Method(GET, POST, PUT, DELETE 등)**으로 표현한다.

1번 사용자에 대해 정보를 받아야 할 때를 예를 들면, 아래와 같은 방법은 좋지 않습니다.

    GET /users/show/1

이와 같은 URI 방식은 REST를 제대로 적용하지 않은 구 버전의 Rails에서 흔히 볼 수 있는 URL입니다. 이 URI은 자원을 표현해야 하는 URI에 /show/ 같은 불필요한 표현이 들어가 있기 때문에 적절하지 않습니다. 본다는 것은 GET이라는 HTTP Method로 충분히 표현할 수 있기 때문이죠. 최근의 Rails는 아래와 같이 변경되었습니다.

    GET /users/1

자원은 크게 `Collection`과 `Element`로 나누어 표현할 수 있으며, 아래 테이블에 기초한다면 서버 대부분과의 통신 행태를 표현할 수 있습니다.

<table>
<caption>RESTful Web Service HTTP methods</caption>
<tr>
<th>Resource</th>
<th>GET</th>
<th>PUT</th>
<th>POST</th>
<th>DELETE</th>
</tr>
<tr>
<th>Collection URI, such as <code>http://example.com/resources/</code></th>
<td>컬렉션에 속한 자원들의 URI나 그 상세사항의 <b>목록</b>을 보여준다.</td>
<td>전체 컬렉션은 다른 컬렉션으로 <b>교체</b>한다.</td>
<td>해당 컬렉션에 속하는 새로운 자원을 <b>생성</b>한다. 자원의 URI는 시스템에 의해 할당된다.</td>
<td>전체 컬렉션을 <b>삭제</b>한다.</td>
</tr>
<tr>
<th>Element URI, such as <code>http://example.com/resources/item17</code></th>
<td>요청한 컬렉션 내 자원을 <b>반환</b>한다.</td>
<td>해당 자원을 <b>수정</b>한다.</td>
<td>해당 자원에 귀속되는 새로운 자원을 <b>생성</b>한다.</td>
<td>해당 컬렉션내 자원을 <b>삭제</b>한다.</td>
</tr>
</table>

 * 이 외에도 **PATCH** 라는 HTTP Method에도 주목하시기 바랍니다. PUT이 해당 자원의 전체를 교체하는 의미를 지니는 대신, [PATCH](http://www.rfc-editor.org/rfc/rfc5789.txt)는 일부를 변경한다는 의미를 지니기 때문에 최근 update 이벤트에서 PUT보다 더 의미적으로 적합하다고 평가받고 있습니다. Rails도 [4.0부터 PATCH가 update 이벤트의 기본 Method로 사용될 것이라 예고](http://weblog.rubyonrails.org/2012/2/26/edge-rails-patch-is-the-new-primary-http-method-for-updates)하고 있습니다.

## 입력 Form은 어떻게 받아오게 하지?

위의 예시를 통해 많은 행태를 표현할 수 있습니다만 새로운 아이템을 작성하거나 기존의 아이템을 수정할 때 작성/수정 Form은 어떻게 제공할지에 대한 의문을 초기에 많이 가집니다.

정답은 **Form 자체도 정보로 취급해야 한다는 것**입니다. 서버로부터 "새로운 아이템을 작성하기 위한 Form을 GET한다"고 생각하시면 됩니다. Rails 에선 기본적인 CRUD를 제공할 때 아래와 같은 REST 인터페이스를 구성해줍니다.

<table>
    <tr>
        <th><span class="caps">HTTP</span> Verb </th>
        <th>Path            </th>
        <th>action </th>
        <th>used for                                   </th>
    </tr>
    <tr>
        <td><span class="caps">GET</span>          </td>
        <td>/photos           </td>
        <td>index    </td>
        <td>display a list of all photos                 </td>
    </tr>
    <tr>
        <td><span class="caps">GET</span>          </td>
        <td>/photos/new       </td>
        <td>new      </td>
        <td>return an <span class="caps">HTML</span> form for creating a new photo </td>
    </tr>
    <tr>
        <td><span class="caps">POST</span>         </td>
        <td>/photos           </td>
        <td>create   </td>
        <td>create a new photo                           </td>
    </tr>
    <tr>
        <td><span class="caps">GET</span>          </td>
        <td>/photos/:id       </td>
        <td>show     </td>
        <td>display a specific photo                     </td>
    </tr>
    <tr>
        <td><span class="caps">GET</span>          </td>
        <td>/photos/:id/edit  </td>
        <td>edit     </td>
        <td>return an <span class="caps">HTML</span> form for editing a photo      </td>
    </tr>
    <tr>
        <td><span class="caps">PUT</span>          </td>
        <td>/photos/:id       </td>
        <td>update   </td>
        <td>update a specific photo                      </td>
    </tr>
    <tr>
        <td><span class="caps">DELETE</span>       </td>
        <td>/photos/:id       </td>
        <td>destroy  </td>
        <td>delete a specific photo                      </td>
    </tr>
</table>

## 모바일 환경에 따라 다른 정보를 보여줘야 한다면?

접속하는 환경에 따라 다른 정보를 보여줘야 할 때가 있습니다. 가령 모바일 디바이스에서 볼 때 다른 사용자 인터페이스를 제공한다든지 하는 경우인데요. 일부 애플리케이션은 독립적인 모바일 웹서비스를 개발한 후 단지 이를 이동시켜주기만 할 때가 있는데, 이는 어떤 경우에 좋지 못한 사용성을 보여줍니다. 모바일 뷰와 일반 웹페이지 뷰의 URI가 달라서 같은 정보를 공유할 때 각 환경에 적절한 디자인과 인터페이스로 보이지 않기 때문입니다.

모바일에서 블로그를 구경하던 도중, 컴퓨터를 이용하고 있는 친구에게 자신이 보고 있는 내용을 보내주고 싶을 때가 있습니다. 티스토리 블로그는 모바일 뷰의 URI가 기존 URI와 달라서, 친구가 해당 URI를 데스크탑에서 열어도 모바일에 최적화된 정보를 받을 수밖에 없게 됩니다. [이 URI](http://littlegold.tistory.com/m/293)를 데스크탑에서 열어보시기 바랍니다.

REST 하게 만든다면 **URI는 플랫폼 중립적**이어야 하며, 정보를 보여줄 때 여러 플랫폼을 구별해야 한다면 `Request Header`의 `User-Agent` 값을 참조하는 것이 좋습니다. 예를 들어 iPhone에서 보내주는 User-Agent 값은 아래와 같습니다.

    Mozilla/5.0 (iPhone; U; CPU like Mac OS X; en) AppleWebKit/420+ (KHTML, like Gecko) Version/3.0 Mobile/1A543a Safari/419.3

대부분 브라우저, OS 플랫폼은 HTTP Request를 보낼 시 보내는 주체에 대한 설명을 User-Agent에 상세하게 포함하여 통신하고 있기 때문에 요청자의 환경을 정확히 알 수 있습니다.


## 버전과 정보 포맷을 지정할 수 있게 해야 한다면?

오픈 API를 제공하거나, 클라이언트가 항상 최신 버전을 유지할 수 없는 환경이라면 서버에서 버전 협상을 지원해야 합니다. 서버가 버전 협상을 지원한다면 최신 버전으로 업데이트가 되더라도 구 버전의 정보 요청에 하위 호환하게 하여 서비스 적용범위를 넓게 유지할 수 있습니다. 이와 함께 클라이언트가 html로 정보를 받을지, json으로 받을지, xml로 받을지 선택할 수 있다면 더욱 좋을 것입니다.

Header의 `Accept` 헤더를 이용해서 요청 환경에서 정보의 버전과 포맷을 지정할 수 있게 합니다. 아래는 Github API에 요청 시 쓰는 Accept 헤더입니다.

    application/vnd.github+json

`vnd.`는 Vendor MIME Type으로, 서비스 개발자가 자신의 독자적인 포맷을 규정할 수 있게 HTTP 표준에서 제공하는 접두어입니다. vnd. 이후에 서비스 제공자 이름을 쓴 후, +로 문서의 기본 포맷을 표현해줍니다.

이에 더해, Accept 헤더는 파라미터를 받을 수 있습니다. 많은 REST 지지자들은 이 파라미터를 이용해 버전 명을 지정하는 것을 권장합니다.

    vnd.example-com.foo+json; version=1.0


## Ajax와 REST

최근 빠른 속도의 웹서비스를 구현하기 위해 서비스 전체를 Ajax 통신으로 구동되게끔 HTML5 애플리케이션을 만드는 일이 많습니다. 서비스 전체를 Ajax 기반으로 구동되게 개발한다면 중복된 콘텐츠를 여러 번 전달하지 않아도 되고, 브라우저 렌더링 과정이 간소화되므로 더욱 빠른 서비스를 구축할 수 있습니다. 하지만 Ajax 기반의 서비스는 초기에 URL에 관련된 문제가 있어 REST한 서비스를 만들 때 애로사항이 있었습니다. 콘텐츠가 바뀌어도 URL은 그대로여서 친구에게 내가 보고 있는 콘텐츠를 보여줄 방법이 불편했기 때문이죠.

최근엔 두 가지 방법으로 이를 보완할 수 있습니다. 첫 번째는 `#!` 기법으로, 구형 브라우저에서도 # 이하의 URL을 Javascript로 자유자재로 변경할 수 있다는 점을 이용한 방법입니다. 방법은 아래와 같습니다.

  1. Ajax 통신을 통해 이동되는 페이지의 URI는 현재 URI의 #! 이후에 붙인다.
  2. 페이지가 처음 열릴 때, #! 이후로 URI가 붙어있다면 해당 URI로 redirect를 해준다.

이와 같은 방법으로 Ajax 서비스를 만들면, 페이지를 이동한 이후에 URL을 친구에게 복사해서 전달해주어도 친구가 내가 보고 있는 콘텐츠를 볼 수 있으며, 구글에서 수집할 때 해당 #! 이하의 URL을 판별해서 제대로 수집해주기 때문에 검색엔진에도 성공적으로 노출될 수 있습니다.

하지만 위 방법의 단점은 1. 상대방이 Javascript를 지원하지 않는 브라우저를 이용하거나 Javascript 기능을 꺼 놓았을 때 제대로 된 콘텐츠를 볼 수 없다는 것이며, 2. URI가 몹시 보기 지저분해진다는 것입니다. 두 번째 방법은 `pushState`라는 새로운 표준을 이용한 방법으로, javascript의 pushState를 통해 Ajax 통신 후에 변경된 컨텐츠의 URI을 제대로 바꿔줄 수 있습니다. 하지만 최신 표준을 지원하는 브라우저에서만 정상적으로 구동되기 때문에, 하위 호환에 신경을 써야 한다는 단점이 있습니다. [pjax](https://github.com/defunkt/jquery-pjax)같은 프로젝트들이 하위 호환을 포함하여 이런 구현을 쉽게 하도록 도와주고 있습니다.


## 언어

언어별로 다른 URI의 서비스를 가지는 서비스들을 종종 볼 수 있습니다. 역시 좋지 못한 설계입니다. 한국어로 작성된 컨텐츠를 보고 있는 중 해당 콘텐츠를 미국인 친구에게 보여줄 일이 생겼다고 가정해봅시다. 단순히 URI를 복사해서 주는 것으로는 미국인 친구에게 내가 보고 있는 정보를 제대로 전달해줄 수 없다면 아주 불편할 것입니다.

Request Header의 `Accept-Language`는 받고자 하는 언어를 명시하고 있습니다. 대부분 브라우저, OS 플랫폼은 사용자가 즐겨 쓰는 언어를 이 Header에 포함하여 요청을 만들고 있기 때문에, 해당 Header를 참조하여 그에 걸맞은 언어를 제공해주는 것이 가장 정확한 서비스를 제공해줄 수 있습니다.

물론 이 방법만으로 부족한 점이 있습니다. 자신의 주 언어와 다른 언어의 세팅을 가지고 서비스를 이용하는 사용자도 있으며, 몇 가지 이유 때문에 해당 사이트만, 해당 순간에만 다른 언어로 정해서 보고 싶을 수 있기 때문입니다. 아쉽게도 일반적인 브라우저에서 언어 변경을 하는 인터페이스는 매우 불편하고 찾기 어렵게 되어있기 때문에, 서비스에서 이에 대한 추가 인터페이스를 제공해주지 않으면 일부 사용자를 잃거나 불편하게 할 수 있습니다. 보통은 Accept-Language보다 우선해서 적용하는 언어 옵션을 세션에 저장할 수 있게 하고, 이에 대한 변경 인터페이스를 서비스에서 제공해주는 식으로 문제를 해결할 수 있습니다.

## 마치며

REST는 여러 가지 서비스 디자인에서 생길 수 있는 문제를 최소화해주고, HTTP 프로토콜의 표준을 최대한 활용하여 여러 추가적인 장점을 함께 가져갈 수 있게 해줍니다. 이번 글에서는 REST하지 않은 서버 설계를 통해 생길 수 있는 실질적인 문제들을 제시하고 REST 아키텍처가 이를 어떻게 해결해주는지 함께 보았습니다.

'REST가 완전한 정답이냐?'라고 한다면 이에 대해서는 아직 논의가 남아있습니다. 구형 브라우저가 아직 제대로 지원해주지 못하는 부분이 분명히 있으며 (PUT, DELETE를 사용하지 못하는 점, pushState를 지원하지 않는 점) 브라우저를 통해 테스트할 일이 많은 서비스라면 쉽게 고칠 수 있는 URL보다 Header 값은 왠지 더 어렵게 느껴지기도 합니다.

만일, 만들고 있는 서비스의 API가 널리 쓰여야 한다면 REST를 완전하게 적용한 디자인이 더 독이 될 수 있습니다. 많은 개발자는 별로 똑똑하지 못하며, HTTP 프로토콜에 대한 이해가 부족하여서 API가 어렵게 느껴질 수 있기 때문입니다. 그러므로 Google을 포함한 많은 기업의 서비스 API가 REST 스타일을 완전히 따르고 있진 않습니다.

하지만 그럼에도 REST가 중요한 점은, 이를 제대로 구현하는 것이 서비스 디자인에 큰 부가이익을 가져다 줄 수 있으며, 많은 현대의 API들이 REST를 어느 정도로 충실하게 반영하느냐를 고민할 뿐이지 REST를 중심으로 디자인되고 있다는 점은 분명하기 때문입니다. REST를 얼마나 반영할 지는 API가 어떤 개발자를 범위에 두는지, 개발 기간이 얼마나 되는지, 함께 하는 동료의 역량은 어떠한지 등을 고려해서 집단마다 다르게 반영하게 될 것입니다.
