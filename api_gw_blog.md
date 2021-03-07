
# API Gateway 이해
## API Gateway 란?
API Gateway 의미를 생각하기전에 API와 Gateway의 각각의 의미를 먼저 찾아보았습니다.

-  **API : 프로그램들이 서로 상호작용하는 것을 도와주는 매개체 역할로  API를 통해 서비스나 데이터를 받을 수 있음**
- **Gateway : 관문 또는 출입구라는 뜻으로 사용자가 현재 네트워크에서 다른 네트워크로 가기위한 반듯이 거쳐야하는 거점 (ex : 톨게이트)** 

API 개념과 Gateway 개념을 조합하여 생각해보면, **MSA 구조에서 API Gateway 는 사용자가 현재 속한 네트워크에서 API를 통해 서비스나 데이터를 받기위해 다른 네트워크를 무조건 거쳐가야하는 곳**을 의미한다고 생각합니다.    

## API Gateway 의 필요성
![API Gateway 미도입](https://github.com/angel20123/blogtest/blob/master/api_gateway01.png?raw=true)

Micro Service Architechture 는 Monolithic Architechture를 여러개의 비지니스 로직을 모듈화하여 잘게 쪼개기 때문에, 서비스 갯수로 보면 전자가 후자보다 훨씬 더 많습니다.  따라서 사용자(클라이언트)가 직접 호출하는 경우에 여러가지 문제점이 발생할 수 있습니다.

 - **사용자(클라이언트)가 모든 엔드포인트를 알고 있어야 서비스를 호출이 가능**
 - **각 서비스 별로 인증/인가를 구현해야하며, 공통 로직(인증/인가 등)이 각 서비스마다 존재하게 됨**
 - **전체 API 별 통계나 API 별 과금이 힘듬**

API Gateway는 사용자가 서비스를 이용하기 위해 모든 엔드포인트를 알 필요가 없이 API Gateway만 알고 있으면, API Gateway가 알아서 각각의 서비스로 라우팅을 해주어 사용자가 서비스를 받을 수 있습니다. 그리고 인증/인가와 같은 공통 로직 처리를 API Gateway에서 처리할 수 있고, API 별 통계나 과금 처리를 API Gateway에서 처리할 수 있습니다. 

![API Gateway 도입](https://github.com/angel20123/blogtest/blob/master/apt_gateway03.png?raw=true)


비단, API Gateway는 MSA 구조에서만 고려대상이 아니라, 기존 Monolithic Architechture 구조에서도 서비스가 많은 경우 사용 될 수도 있을 것 같습니다.

## API Gateway 주요 기능

**1. 인증/인가**
API Gateway가 없는 경우 각 서비스별로 인증과 인가 로직을 구현하여야 합니다.  보통 인증과 인가로직은 공통 로직으로 각 서비스 수만큼 공통로직이 중복이 발생하게 되고, 서비스가 많아지면 유지보수가 힘들어집니다. 따라서 사용자의 모든 요청을 처음 받는  API Gateway에 인증과 인가로직을 구현함으로서 공통로직을 줄일 수 있고, 유지보수도 그 만큼 쉬워지게 됩니다.

**2. 엔드포인트 단순화**
사용자는 API Gateway 엔드포인트만 알고 있으면, 인증과 인가된 경우 전체 서비스에 접근할 수 있습니다.
 
 **3. 라우팅과 로드밸런싱**
 API Gateway는 사용자가 요청한 데이터를 보고 적절한 서비스로 라우팅해주는 기능을 가지고 있습니다. 그리고 Monolithic Architechture 에서 L4를 통해 로드밸런싱이 가능했던것 처럼 API Gateway를 통해 로드밸런싱이 가능합니다. 차이점은 API Gateway 경우 실제 요청이 많은 서비스에 대해서만 로드밸런싱을 수행한다거나 하는 스케일 업-아웃의 자유도가 높다고 볼 수 있습니다. 
 
**4. 서비스 디스커버리**
서비스가 추가되고 제외되는 일이 잦은 MSA 구조 특성상 필요한 기능 중 하나 입니다. 서비스의 IP와 포트 주소를 찾아주는 역할을 하는 기능입니다. 서비스가 적은 경우 수동으로 찾아서 등록할 수 있겠지만, 서비스가 많은 경우 수동은 불가능하기 때문에 서비스 디스커버리 기능이 필수 기능으로 작동해야 할 것입니다.

> 쿠버네티스 환경(or 클라우드)에서는  IP 주소 값이 고정이 아니기때문에 특히 더 필요한 기능일 것입니다. 

**5. API 별 통계와 과금**
사용자는 API Gateway를 통해 모든 요청을 하기때문에 API Gateway에서 필요한 통계를 저장, 모니터링 할 수 있습니다. 이렇게 저장된 데이터를 가지고 이후 각 사용자별로 API 사용량을 가지고 과금을 할 수 있습니다.


## API Gateway 종류
나름 많이 알려진 프로젝트에 도입할 수 있을법한 API Gateway 3종에 대해 간단하게 소개해보려고 합니다.  자세한 내용은 인터넷에서 찾아보시길 바랍니다.

**1. Kong**

![Kong Gateway](https://github.com/angel20123/blogtest/blob/master/kong2.png?raw=true)
<center> ( 출처 : https://www.tothenew.com/blog/getting-started-with-application-authentication-via-kong-api-gateway/ ) </center>

Kong API Gateway는 **nginx 기반**으로 **플랫폼에 구애받지않고** 설치가능하며, **플러그인(모듈) 형태**로 기능을 확장가능합니다.  만약 Custom 한 기능을 추가하기 위해서는 **Lua** 라는 Script Language를 이용해 **3rd part 플러그인을 개발**하여 Kong API Gateway에 추가해야합니다. 
그리고 화면을 제공(konga설치필요)하여 개발자가 아니더라도 일반 운영자가 쉽게 라우팅 설정을 할 수 있습니다.


**2. Zuul**

![Zuul API Gateway](https://github.com/angel20123/blogtest/blob/master/zuul.png?raw=true)
<center> ( 출처 : https://github.com/Netflix/zuul ) </center>

MSA를 잘 활용하고 있는 **Netflex사의 API Gateway를 오픈소스화** 한 것이 Zuul API Gateway 입니다. **groovy 언어**로 작성된 다양한 Filter 를 실행하며, 추가적인 기능은 **Filter를 정의**해서 사용합니다. (Filter 기반) 
Zuul 모듈을 가지고 직접 구현하는 것은 러닝커브가 커서 어려우나, Spring 진형에서 진행했던(과거형) **spring cloud netflix 프로젝트**를 이용하여 기능 구현하면 몇 가지 설정과 어노테이션만으로 Zuul API Gateway를 실행해 볼 수 있습니다. 

> groovy 는 자바에 파이썬, 루비, 스몰토크 등의 특징을 더한 동적 객체 지향 프로그래밍 언어입니다. 스크립트 파일 그대로 실행 할 수도 있고, 자바처럼 컴파일하여 쓸 수도 있습니다.

**2. Spring Cloud Gateway (SCG)**

![SCG API Gateway](https://github.com/angel20123/blogtest/blob/master/spirngCloudGateway.jpg?raw=true)
<center> ( 출처 : https://dev.to/only2dhir/an-introduction-to-spring-cloud-gateway-3o89 ) </center>

**Srping 진형**에서 프로젝트로 진행되고 있는 API Gateway입니다. Spring Cloud Gateway(SCG)는 **논블로킹** **비동기방식**으로 **Netty 서버**위에서 동작합니다. 따라서 서블릿 컨테이너 또는 War 로 빌드하게 되면 동작하지 않습니다. Zuul 과 비슷하게 **Filter로 정의해서 사용**합니다. (Filter 기반) 

> **API Gateway 선택은 프로젝트 성격에 맞게 선택하시기 바라며, 구글에 보면 Kong 기반인 Nginx, Zuul, SCG 성능비교 글이 많이 있습니다. 참고하시어 선택하시면 될 듯 합니다.**

### Zuul 관련 이슈 사항
아래 모듈들은 Spring boot 2.4 버전부터(# Spring Cloud 2020.0 Release Notes) 확정된 내용입니다. Netflix 사에서 유지보수 개발만 진행되는 라이브러리에 대해서 Spring 진형에서 지원하지 않겠다고 발표하였습니다. 따라서 Spring boot 2.4 버전에서 쓰기 힘들것으로 보입니다. 다만 아래에 포함되지않은 모듈인 Eureka는 빠져있어 Eureka는 계속 지원할 것으로 보입니다. 

|더 이상 지원하지 않는 모듈                             |
|----------------------------------------------------|
|spring-cloud-netflix-archaius                       |
|spring-cloud-netflix-concurrency-limits             |
|spring-cloud-netflix-dependencies                   |
|spring-cloud-netflix-hystrix                        |
|spring-cloud-netflix-hystrix-contract               |
|spring-cloud-netflix-hystrix-dashboard              |
|spring-cloud-netflix-hystrix-stream                 |
|spring-cloud-netflix-ribbon                         |
|spring-cloud-netflix-sidecar                        |
|spring-cloud-netflix-turbine                        |
|spring-cloud-netflix-turbine-stream                 |
|spring-cloud-netflix-zuul                           |
|spring-cloud-starter-netflix-archaius               |
|spring-cloud-starter-netflix-hystrix                |
|spring-cloud-starter-netflix-hystrix-dashboard      |
|spring-cloud-starter-netflix-ribbon                 |
|spring-cloud-starter-netflix-turbine                |
|spring-cloud-starter-netflix-turbine-stream         |
|spring-cloud-starter-netflix-zuul                   |
|spring-cloud-starter-netflix-hystrix-dashboard      |





> Written with Jongtae Jeong(Jong_tae.Jeong@kt.com).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTIyNTI0NDQzNiwyNzEzNzg0MDYsNTQ4MT
E3MzIzLDEwNTM5NDM4MzgsMTM2MzQ5NDQ1LC00OTMxNzY4OTEs
LTMyMTAxOTM5MiwxMjA0MjkwMzQyLC00Mzk2NzczOTEsLTE0Nj
I5NzY0MCwxMDU5ODY3MDEzLC0xNzQ2NjEyMjg0LDQ3MzE3MDA0
NSwtOTE2MTI5OTM0LDE3ODk1MjcyMjddfQ==
-->