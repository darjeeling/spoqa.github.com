---
layout: entry
title: 스포카 서버의 구조
author: 문성원
author-email: longfin@spoqa.com
description: 스포카 서버의 구조와 사용된 기술들에 대해서 알아봅니다.
publish: true
---

안녕하세요. [스포카 개발팀]에서 서버 관련 개발 업무를 담당하고 있는 문성원입니다. 오늘은 스포카 서버의 구조와 사용된 기술들에 대해서 함께 살펴보겠습니다. 

# 스택이란?
---
먼저 스택(Stack)이란 용어에 대해서 함께 생각해보죠. 컴퓨터 과학을 공부하신 분들이라면 선입후출(FILO)이나 스택 오버플로우(Stack Overflow)등의 개념으로 익숙하실만한 용어기도 합니다. 그런데 서버 구조를 설명한다면서 왠 스택이냐구요? 다행히(?)도 지금부터 살펴 볼 스택은 [솔루션 스택(Solution Stack)]입니다. 스포카 서버라는 큰 솔루션이 원활히 동작하기 위해서 쓰이고 있는 각종 서브 시스템과 컴포넌트들의 묶음을 이야기하는 것으로 바꿔말하자면 이 글에서 다룰 기술 이야기는 모두 이 스택에 관한 이야기입니다.

2011년 12월 현재 스포카 서버를 구성하고 있는 스택은 다음과 같습니다.

* [Dotcloud]
	* [Linux] 2.6.38.2
	* [nginx] 0.8.53
	* [uwsgi] 0.9.8.5
	* [Python] 2.6.5
	* [Redis] 2.2.2
	* [Celery] 2.2.7
* [Amazon Relational Database Service][Amazon RDS]
	* [MySQL] 5.5.12 
* [Amazon Simple Storage Service][Amazon S3]


# [Dotcloud]
---
[Dotcloud]는 지금부터 설명드릴 스택을 묶어서 제공해주는 [PaaS(Platform as a Service)][PaaS]의 일종입니다. [Amazon Elastic Cloud Computing(Amazon EC2)][Amazon EC2] 기반으로 동작하며 거기에 더해 **손쉬운 확장과 배포**가 장점입니다. 스포카 서버는 데이터베이스([Amazon RDS])와 업로드되는 데이터([Amazon S3]) 이외의 모든 서비스를 [Dotcloud]를 통하여 제공하고 있습니다.

# [nginx], [uwsgi]. 그리고 [WSGI]
---
기본적으로 스포카 서버는 [HTTP] 형식의 요청을 받아 응답을 돌려주는 웹 어플리케이션입니다. 이러한 처리는 1차적으로 [nginx]를 통해 이뤄지는데, 이 중 서버사이드에서 처리가 필요한 경우에는 [uwsgi]라는 데몬이 이 처리를 담당합니다. (구버젼의 [Apache Tomcat]을 사용하시던 [Java] 개발자분들은 [Apache Tomcat]과 [Apache httpd]와의 관계를 떠올리시면 편합니다.)

이 경우 [uwsgi]는 일종의 **어플리케이션 컨테이너(Application Container)**로 동작하게 됩니다. 적재한 어플리케이션을 실행만 시켜주는 역할이죠. 이러한 [uwsgi]에 적재할 어플리케이션(스포카 서버)에는 일종의 규격이 존재하는데, 이걸 [WSGI]라고 합니다.(정확히는 [WSGI]에 의해 정의된 어플리케이션을 돌릴 수 있게 설계된 컨테이너가 [uwsgi]라고 봐야겠지만요.) [WSGI]는 [Python] 표준([PEP-033](http://www.python.org/dev/peps/pep-0333/))으로 [HTTP]를 통해 요청을 받아 응답하는 어플리케이션에 대한 명세로 이러한 명세를 만족시키는 클래스나 함수, ([\_\_call\_\_](http://docs.python.org/reference/datamodel.html#object.__call__)을 통해 부를 수 있는)객체를 **WSGI 어플리케이션**이라고 합니다.

정리하자면 스포카 서버는 [WSGI]에 맞게 작성된 프로그램을 [nginx]와 [uwsgi]를 통해 운용하여 요청을 처리하는 웹 어플리케이션이라고 할 수 있습니다.

# [Redis]
---
[Redis]란 **키-값(Key-Value) 저장 서버**로 확장이 용이하며 속도가 우수합니다. 스포카 서버에선 이를 **내부적인 임시 데이터 관리**와 [Celery]의 **작업(Task) 분배**에 사용하고 있습니다. 

# [Celery]
---
[Celery]는 [Python]으로 작성된 **비동기 작업 큐(Asynchronous task queue/job queue)**입니다. 앞서 소개한 **작업(Task)**를 **브로커(Broker, 스포카 서버는 [Redis]를 사용)**를 통해 전달하면 하나 이상의 **워커(Worker)**가 이를 처리하는 구조입니다. 포인트 적립-공유에 따른 분배처리, 포스팅 기능, 페이스북/트위터 공유등의 비동기 처리가 필요한 작업을 [Celery]에 위임하여 처리하고 있습니다.

# [Amazon Relational Database Service][Amazon RDS]
---
대부분의 웹 어플리케이션과 마찬가지로 스포카 서버는 영속적으로 저장되어야하는 정보(회원 목록, 구매 내역)들을 디스크 기반의 데이터베이스(Database)에 저장합니다. [Amazon Relational Database Service(Amazon RDS)][Amazon RDS]는 [Amazon EC2]를 기반으로 그러한 데이터베이스를 간편하게 관리(모니터링, 백업, 접근제어)할 수 있게 도와주는 웹서비스입니다. [Oracle]과 [MySQL]을 지원하는데 스포카 서버는 그 중 [MySQL]을 사용하고 있습니다. 

# [Amazon Simple Storage Service][Amazon S3]
---
[Amazon Simple Storage Service(Amazon S3)][Amazon S3]는 [Amazon RDS]와 마찬가지로 [Amazon EC2]를 기반으로 한 데이터 저장 관리 서비스입니다. 스포카 서버에 업로드 되는 사진이나 문서등의 파일들을 통합하여 관리하여 서버의 인스턴스를 늘려 확장하는 경우에도 문제없이 대처할 수 있도록 하는 것이 주 목적입니다.

[스포카]: http://spoqa.com
[스포카 개발팀]: http://spoqa.github.com
[솔루션 스택(Solution Stack)]: http://en.wikipedia.org/wiki/Solution_stack
[Dotcloud]: http://dotcloud.com
[Linux]: https://github.com/torvalds/linux
[nginx]: http://nginx.org
[uwsgi]: http://projects.unbit.it/uwsgi
[Python]: http://python.org/
[Redis]: http://redis.io/
[Celery]: http://celeryproject.org
[PaaS]: http://en.wikipedia.org/wiki/Platform_as_a_service
[Amazon Web Services(AWS)]: http://aws.amazon.com/
[Amazon S3]: http://aws.amazon.com/s3/
[Amazon RDS]: http://aws.amazon.com/rds/
[MySQL]: http://www.mysql.com/
[HTTP]: http://www.w3.org/Protocol
[Java]: http://www.java.com/
[Apache Tomcat]: http://tomcat.apache.org/
[Apache httpd]: http://httpd.apache.org/
[WSGI]: http://www.wsgi.org/en/latest/index.html
[Amazon EC2]: http://aws.amazon.com/ec2/
[oracle]: http://www.oracle.com/kr/products/database/index.html
