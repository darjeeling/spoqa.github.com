---
layout: entry
title: Jinja2 API를 이용한 템플릿 분석
author: 문성원
author-email: longfin@spoqa.com
description: 템플릿 언어 Jinja2의 동작 방식과 지원하는 API를 통해 작성되어 있는 템플릿들의 구문을 분석하는 방법을 알아봅니다.
---

스포카 개발팀 문성원입니다. 저희는 웹서비스를 개발할때 템플릿 언어(Template Language)로 [Jinja2]를 [사용하고 있습니다](). 오늘은 이런 [Jinja2]의 동작 방식을 알아보고, [Jinja2의 API](http://jinja.pocoo.org/docs/api/)를 이용해서 템플릿에 쓰인 구문을 분석하는 방법을 함께 알아보려 합니다.

# Warming up

[Jinja2]의 동작방식은 아주 간단합니다. 뼈대가 되는 템플릿 파일(<code>.html</code>, <code>.htm</code> 등)을 읽어서 이름과 값들이 묶여있는 [환경(Environment)](http://jinja.pocoo.org/docs/api/?highlight=environment#jinja2.Environment)에서 평가한 뒤, 이를 다시 문자열로 반환합니다. 이렇게 말이죠.

<script src="https://gist.github.com/3000690.js?file=template.py"></script>

<script src="https://gist.github.com/3000690.js?file=template.html"></script>

 보통 [Jinja2]는 [같은 제작자(Armin Ronacher)](http://lucumr.pocoo.org/about/)가 작성한 다른 웹 프레임워크인 [Flask]와 같이 사용되곤 합니다.(물론 위의 예제처럼 [Flask]의 도움이 없이도 잘 작동하지만요.) [Flask]는 [<code>app.jinja_env</code>](http://flask.pocoo.org/docs/api/#flask.Flask.jinja_env)와 같은 멤버 변수나 <code>tojson</code> 같은 [확장 필터(filter)](http://flask.pocoo.org/docs/templating/#standard-filters)를 통해 기본적으로 [Jinja2]를 [잘 지원합니다.](http://flask.pocoo.org/docs/templating/) 

<script src="https://gist.github.com/3000690.js?file=flask_template.py"></script>

# Our problems

 다음과 같은 템플릿을 생각해보죠. 현재 접속한 관리자(<code>current_admin</code>)의 권한(평문(Plain Text)로 된 리스트입니다.)을 체크(<code>current\_admin.has\_permission()</code>)해서 권한별로 보여줄 항목을 가리는 템플릿입니다.

<script src="https://gist.github.com/3000690.js?file=some.html"></script>

 사실 이 템플릿 자체가 문제는 아닙니다. 이런 방식은 간단하게 무언가를 만들어서 적용하기(quick and dirty)에는 괜찮습니다. 문제는 이러한 템플릿이 계속 늘어날 거라는 거죠. 기능이 늘어날때마다 템플릿도 늘어날 것이고, 템플릿이 늘어날때마다 권한도 늘어날 것이고, 권한이 늘어날때마다 그 권한을 나타내는 식별자도 늘어날 것입니다. 아무래도 뒷감당이 쉽지는 않아보이지요. 

 그래서 생각한 것이 '먼저 템플릿에 텍스트로 적고 나중에 모아서 정리하는 방식'이었습니다. 그러려면 템플릿에 적힌 다음과 같은 코드들을 전부 모아서 볼 수 있는 모종의 함수(<code>find\_all\_permission_calls()</code>)가 필요하죠. 그런 함수를 만들 수나 있냐구요? 당연하죠.

# Find template

 그럼 <code>find\_all\_permission_calls()</code>과 같은 함수를 만들려면 어떻게 해야할까요? 우선 처리할 모든 템플릿을 찾아야합니다. 

 일반적으로 [Flask]는 <code>template</code> 디렉토리에 템플릿 파일들을 모아 놓는 것이 기본이므로 거길 찾아도 되겠습니다만, 우아하진 않습니다. 이럴때 사용해볼만한 것이 [<code>environment.list_templates()</code>](http://jinja.pocoo.org/docs/api/#jinja2.Environment.list_templates) 입니다. 지정된 환경에 등록된 템플릿을 모두 돌려주는 지금 목적에 딱 맞는 함수죠. [Flask] 애플리케이션이니 <code>app.jinja_env</code>를 이용하면 됩니다.
 
 <script src="https://gist.github.com/3000690.js?file=find_templates.py"></script>

# Parse

 자 이제 템플릿들을 모두 찾았으니 처리하기만하면 됩니다. 어떤 방법이 있을까요?
 
 가장 먼저 떠오르는 것은, 파일들을 읽어서 <code>.has\_permission</code> 따위의 문자열이 있는지를 보는 것입니다. 이런 단순 문자열 매칭도 썩 나쁜 방법은 아니지만 우리가 <code>.has\_permission</code>이 우리가 찾는 함수 호출인지 아니면 다른 문자열(이를테면 CSS의 클래스명이랄지.)인지 분간할 수 없다는 치명적인 문제가 있습니다. 
 
이쯤에서 "[Jinja2]는 그럼 어떻게 저런 문자열을 처리하지?" 라는 생각이 드시는 분들도 계실겁니다.(저 역시 그랬구요.) 템플릿 언어인 [Jinja2]는 다른 여타 프로그래밍 언어처럼 [문자열을 읽어서 토큰 단위로 자르는 기능(Lexer)](http://en.wikipedia.org/wiki/Lexical_analysis)과 잘린 토큰들을 분석해서 [AST(Abstract Syntax Tree)](http://jinja.pocoo.org/docs/extensions/#ast)로 [만들어 내는 기능(Parser)](http://en.wikipedia.org/wiki/Parser)를 갖추고 있습니다. 더욱이 이런 기능들을 [API로 제공](http://jinja.pocoo.org/docs/api/#low-level-api)하기 때문에 사용자가 직접 호출하는 것도 가능하죠. 이렇게 말입니다.

<script src="https://gist.github.com/3000690.js?file=parse_template.py"></script>

# In the tree...

 이렇듯 템플릿을 AST로 변환할 수 있다면 순회할 수도 있을까요? 물론입니다. Jinja2의 AST는 템플릿에서 문법적으로 의미를 가지는 덩어리인 [노드(Node)](http://jinja.pocoo.org/docs/extensions/#jinja2.nodes.Node)들의 묶음으로, 표현되는데  [<code>find_all()</code>](http://jinja.pocoo.org/docs/extensions/#jinja2.nodes.Node.find_all)을 이용하면 원하는 유형의 노드만 골라낼 수 있습니다. 우리는 <code>has_permission()</code>이란 함수에 대한 호출을 찾고 있으므로 찾아야할 노드 유형은 [콜(Call)](http://jinja.pocoo.org/docs/extensions/#jinja2.nodes.Call)이 됩니다. 

<script src="https://gist.github.com/3000690.js?file=find_call_nodes.py"></script>

 이제 모든 호출 유형 중에서 내부적으로 <code>has_permission</code>이라는 [속성에 접근하는 노드(GetAttr)](http://jinja.pocoo.org/docs/extensions/#jinja2.nodes.Getattr)가 있는지를 확인해서 돌려주면 끝입니다. 최종적인 코드 얼개는 이렇습니다.

<script src="https://gist.github.com/3000690.js?file=final.py"></script>

# More and more

사실 위에서 사용한 저수준 API를 이용하면 아예 [새로운 태그를 정의하는 일](http://jinja.pocoo.org/docs/extensions/#module-jinja2.ext)도 가능합니다. 심지어 [Jinja2]에 기본 내장된 [국제화 확장(i18n extension)](https://github.com/mitsuhiko/jinja2/blob/master/jinja2/ext.py#L153)조차도 이러한 저수준 API를 통해 구현되어 있죠. 관심있으신 분들은 참고해보시기 바랍니다.

---

[Jinja2]: http://jinja.pocoo.org/
[Flask]: http://flask.pocoo.org/
