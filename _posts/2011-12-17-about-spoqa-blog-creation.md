---
layout: entry
title: Spoqa 블로그 탄생 비화
author: akaz
author-email: akaz@spoqa.com
description: 스포카 블로그는 어떤 기술을 통해 만들어졌을까요? github의 pages서비스와 Jekyll site generator에 대하여 알아봅니다.
---

## github pages and Jekyll
---

안녕하세요. 스포카에서 [안드로이드 플랫폼](http://market.android.com/details?id=com.spoqa) 개발을 담당하고 있는 akaz입니다. 이번에 스포카 블로그를 오픈하게 되면서 제가 블로그에 관한 기술들을 알아보고 실제로 오픈하는 역할을 맡게 되었는데요, 그 과정들을 간단히 소개하고자 합니다.

스포카의 블로그를 운용하기 위해서 [github](https://github.com)의 [pages](http://pages.github.com)서비스를 이용합니다. 그리고 pages서비스에서는 기본적으로 [Jekyll](http://github.com/mojombo/Jekyll)이라는 블로그 자동 생성 프로그램을 제공해주고 있습니다. 글을 쓰고 게시 버튼을 눌러 포스팅하는 [텀블러](http://tumblr.com)나 [블로그 스팟](http://www.blogger.com)같은 일반적인 블로그 사이트와는 다르게 pages에서는 github 저장소에 변경되거나 새로 첨가된 파일을 push함으로써 정적으로 블로그를 생성하고 글을 포스팅 합니다.

## How to make blog inside github?
---

블로그를 생성하기에 앞서 먼저 github저장소를 만들어야 하는데요, github 가입 후 저장소 이름을 <자신의 user id>.github.com으로 생성 후 이 곳에 index.html파일을 하나 푸시하면 10분 정도 뒤에 http://<자신의 user id>.github.com 의 주소로 접속해서 생성된 사이트를 확인할 수 있습니다. 처음에 사이트를 생성하고 설정하느라 시간이 10분정도 소요되는데 그 이후에는 거의 바로 푸시 이후 변경사항을 확인할 수 있습니다. 

## How to use Jekyll?
---

앞서 한 방식대로 html파일을 매번 수정하여 저장소에 넣는 방식으로 블로그를 운영한다면 블로그 운영이 너무 피곤한 작업이 될 것입니다. Jekyll을 사용함으로서 이 과정을 더 편하게 진행할 수 있습니다. Jekyll을 사용하기에 앞서 먼저 [gem을 설치](http://github.com/mojombo/Jekyll/wiki/install)해야 합니다. 

<pre class="terminal"><code>$ gem install jekyll # 권한 문제가 있다면 sudo gem install jekyll </code></pre>

앞서 소개한 링크에 설치 방법이 잘 나와있으므로 설치에 문제가 있다면 링크를 참고하여 설치하고 나면 이제 블로그를 만들기 위하여 로컬에 적당한 폴더하나를 생성하고 \_config.yml파일을 생성하여 블로그 설정 사항을 세팅하면 됩니다. 

\_config.yml의 구조와 세팅값에 대한 설명이 이 [페이지](https://github.com/mojombo/Jekyll/wiki/configuration)에 자세히 나와있습니다. 특별한 상황이 아니면 페이지의 밑에 나와있는 default세팅 값만 적용하면 블로그 생성 시에 큰 문제가 없습니다.

세팅 값을 담은 yml파일을 생성하고 나면 이제 사이트 구조를 만들어야 할 차례입니다. Jekyll의 [Usage](https://github.com/mojombo/Jekyll/wiki/usage)페이지의 **Deploy your site**섹션을 확인하면 Jekyll블로그를 생성하기 위한 사이트 구조를 확인해 볼 수 있습니다.

<pre>
.
|-- _config.yml
|-- _includes
|-- _layouts
|   |-- default.html
|   \-- post.html
|-- _posts
|   |-- 2007-10-29-why-every-programmer-should-play-nethack.textile
|   \-- 2009-04-26-barcamp-boston-4-roundup.textile
|-- _site
\-- index.html
</pre>

이 template폴더들중에 블로그에 **가장 큰 역할을 하는 폴더**를 몇 개 살펴보겠습니다.

**\_layout폴더**에는 말 그대로 **레이아웃과 관련된 파일**이 올라가게 됩니다. 일반적으로 default.html파일 하나 post.html파일 하나를 두게 되는데요, 용도를 살펴보자면 default.html에는 모든 페이지에 **공통적으로 적용될 레이아웃**(가령 header와 footer를 보여준다던지)을 설정하기 위해 사용되고 post.html의 경우 **쓰여진 글들에 적용될 레이아웃**을 지정하기 위해 사용되게 됩니다. \_layout폴더에 들어갈 html파일들안에 [Liquid]문법을 적용할 수 있습니다. 

**\_posts폴더**에는 **블로그에 포스트할 글들**이 올라가게 됩니다. 일반 html파일을 올려도 되고혹은 [Markdown](http://daringfireball.net/projects/markdown/syntax)(확장자 .md)이나 [Textile](http://en.wikipedia.org/wiki/Textile_%28markup_language%29)(확장자 .textile) 문법으로 쓰여진 문서를 올려두면 Jekyll을 이용하여 서버를 시작할 때 자동으로 html파일로 변환되어 \_site폴더에 저장되게 됩니다. 단, \_posts폴더에 올라가는 파일을 올릴때에는 파일 이름에 **약간의 형식**을 맞춰주어 올려야 합니다. 파일의 형식은 YEAR-MONTH-DAY-title.MARKUP의 형태가 되어야 하는데, 예를 들면 2011-12-17-this-posting-title.md같이 파일 이름을 지을 수 있습니다. (이 경우는 마크다운 문법 파일이라서 확장자를 md로 하였고 파일의 확장자는 자신이 원하는 확장자(html, md, textile)로 하면 됩니다. 다만 확장자에 따르는 문법에 맞게 문서를 구성하셔야겠지요?)

## Jekyll과 Liquid문법을 이용하여 실제로 간단한 블로그 만들어보기
---

이제 한 번 실제로 블로그를 만들어볼 차례입니다. 이 섹션에서는 아주 간단한 블로그 하나를 만들어가면서 Jekyll의 사용 방법과 [Liquid]문법에 대하여 설명합니다. Jekyll 설치 후 블로그로 쓸 폴더를 하나 만든 후 그 안에 \_config.yml파일과 index.html파일 그리고 \_layouts, \_posts폴더를 생성합니다. (\_config.yml파일은 앞서 링크에서 제공된 기본 \_config.yml을 사용합니다.)

여기에 사용된 모든 소스들이 이 [저장소](https://github.com/akaz00/akaz00.github.com)에 저장되어 있습니다. 이 파일들을 받고 바로 Jekyll 서버를 실행시키면 [블로그](http://akaz00.github.com/)를 확인해 볼 수 있습니다. 

이제 실제 소스 코드를 보면서 설명하겠습니다.

### template 파일 작성 
---

먼저 view를 담당할 template html파일을 작성해보겠습니다.

**\_layouts/default.html**
<script src="https://gist.github.com/1484815.js?file=gistfile1.py">  </script>

가장 먼저 보게 될 파일은 default.html파일입니다. 이 파일에서 일반적으로 쓰일 html파일의 구조와 레이아웃을 지정해놓고, content class의 division에 \{\{ content \}\}를 [Liquid]마크업을 삽입하여 그 division안으로 앞으로 만들어갈 파일 안의 본문 내용이 삽입되도록 설정하였습니다.  

**index.html**
<script src="https://gist.github.com/1484813.js?file=gistfile1.py">  </script>

그 다음으로 보실 파일은 index.html파일인데요, 맨 위에 대쉬(-) 새개로 덮혀있는 부분을 보실 수 있습니다. 이것은 [YAML Front matter]형식으로 작성된 선언부입니다. layout이라고 적혀있고 default로 되어있는데, 그 말은 \_layouts폴더의 default.html에 content를 집어넣겠다는 선언입니다. 즉, 현재 index.html에 들어가 있는 html문서 내용이 default.html안에 [Liquid] 마크업으로 \{\{ content \}\}라고 쓰여진 부분에 삽입되게 되는것이지요. 

코드 중간을 보시면 포스팅된 글의 목록을 가져오기 위해 중간에 반복문으로 li태그 안에 문서에 접근할 수 있는 링크를 만들어주는 것을 확인할 수 있습니다. post들을 하나 하나 가져오면서 post.date(포스팅 날짜), post.url(실제 문서로 접근할 url) 그리고 post.title(포스팅의 제목)에 접근하여 관련 내용을 list로 만들어주고 있습니다. date, url, title앞에 붙은 post의 정체는 무엇일까요? 그것들은 **Jekyll에서 기본적으로 제공하는 전역 변수들**입니다. Jekyll에서는 이러한 미리 제공되는 [template global data](https://github.com/mojombo/jekyll/wiki/Template-Data)들을 특정한 변수 이름으로 불러올 수 있도록 하고 있습니다.

**\_layouts/post.html**
<script src="https://gist.github.com/1484817.js?file=gistfile1.py">  </script>

포스팅 내용이 표시될 template html파일입니다. 간단히 h1태그 안에 제목을 넣어주었고 역시 \{\{ content \}\}를 삽입하여 그 안에 내용이 삽입되도록 하였습니다. (post 파일의 [YAML Front matter]에 들어간 모든 항목에 접근하기 위해 앞에 page.가 붙습니다, 이 경우 page.title로 접근하여 제목을 얻어왔습니다.) page역시 미리 제공되는 [template global data](https://github.com/mojombo/jekyll/wiki/Template-Data)의 한 변수입니다.

### \_post안의 포스팅 파일 살펴보기
---

**\_posts/2011-12-17-first-post.md**
<script src="https://gist.github.com/1484827.js?file=gistfile1.py">  </script>

이제 포스팅을 하려하는 내용을 담을 md파일을 살펴보겠습니다. layout에는 post.html을 적용할 것이므로 post라 적고 title에는 포스팅의 제목을 적어줍니다. 그리고 마지막으로 [YAML Front matter]아래에 본문 내용을 적어줍니다. 파일 이름은 연도-월-일-제목.확장자의 형식을 지켜주어야 하므로 2011-12-17-first-post.md라 지었습니다.

이상으로 매우 간단한 블로그가 완성되었습니다. 실제 적용되는 모습을 보려면 Jekyll서버를 가동하면됩니다.

<pre class="terminal"><code>$ jekyll --server</code></pre>

명령어를 통해 Jekyll서버를 가동시킵니다. 가동시키면 이제 **\_site폴더가 생성**되고 그 안에 **변환되어진 html파일들이 저장**되는 것을 볼 수 있습니다.

\_config.yml파일에 설정된 디폴트 포트가 **4000**이므로 브라우져를 킨 후 **localhost:4000**주소로 접근하여 확인할 수 있습니다. 

![blog screenshot 1](/images/screenshot1.png)
**index화면의 모습 (index.html)**

![blog screenshot 2](/images/screenshot2.png)
**실제 포스팅으로 들어온 화면의 모습 (post.html)**

완성된 블로그의 모습입니다, 현재는 아무것도 꾸며진게 없고 포스팅도 없어 매우 썰렁하지만 CSS를 이용하여 블로그를 좀 더 예쁘게 만들어  갈 수 있을것입니다!

\_posts안에 있는 post파일들에 들어갈 [YAML Front matter]에 관한 정보는 [이곳](https://github.com/mojombo/Jekyll/wiki/template-data)의 **Post**섹션에서 확인가능합니다.

## Example sites
---

지금까지 개략적으로 github pages와 Jekyll의 사용법을 알아보았습니다. 

Jekyll을 이용하여 만들어진 여러 사이트들을 모아놓은 [리스팅 페이지](https://github.com/mojombo/Jekyll/wiki/sites)의 저장소를 확인하면 더 많은 활용예를 볼 수 있습니다.

또한 이 [사이트](http://net.tutsplus.com/tutorials/other/building-static-sites-with-Jekyll/)에서도 유용한 정보를 확인할 수 있습니다. 

**Link sites**

[github](https://github.com) - github 공식 사이트    
[github pages](http://pages.github.com) - github pages 소개 페이지    
[Jekyll](http://github.com/mojombo/Jekyll) - Jekyll 저장소    
[Jekyll을 이용하여 생성된 블로그 리스트](https://github.com/mojombo/Jekyll/wiki/sites)       
[YAML Front matter](https://github.com/mojombo/Jekyll/wiki/YAML-Front-Matter) - YAML Front matter 소개 페이지    
[Liquid for Designers](https://github.com/Shopify/Liquid/wiki/Liquid-for-Designers) - Liquid 문법 소개 페이지    

[Liquid]: https://github.com/Shopify/Liquid/wiki/Liquid-for-Designers
[YAML Front matter]: https://github.com/mojombo/Jekyll/wiki/YAML-Front-Matter
