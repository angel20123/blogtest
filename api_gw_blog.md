
# API Gateway 이해
## API Gateway 란?
API Gateway 의미를 생각하기전에 API와 Gateway의 각각의 의미를 먼저 찾아보았습니다.

-  API : 프로그램들이 서로 상호작용하는 것을 도와주는 매개체 역할로  API를 통해 서비스나 데이터를 받을 수 있음
- Gateway : 관문 또는 출입구라는 뜻으로 사용자가 현재 네트워크에서 다른 네트워크로 가기위한 반듯이 거쳐야하는 거점       (ex:톨게이트) 

API 개념과 Gateway 개념을 조합하여 생각해보면, **MSA 구조에서 API Gateway 는 사용자가 현재 속한 네트워크에서 API를 통해 서비스나 데이터를 받기위해 다른 네트워크를 무조건 거쳐가야하는 곳**을 의미한다고 생각합니다.    

## API Gateway 의 필요성
![API Gateway 필요성](https://github.com/angel20123/blogtest/blob/master/api_gateway01.png?raw=true)

Micro Service Architechture 는 Monolithic Architechture를 여러개의 비지니스 로직을 모듈화하여 잘게 쪼개기 때문에, 서비스 갯수로 보면 전자가 후자보다 훨씬 더 많습니다.  따라서 사용자(클라이언트)가 직접 호출하는 경우에 여러가지 문제점이 발생할 수 있습니다.

 - 사용자(클라이언트)가 모든 엔드포인트를 알고 있어야 서비스를 호출이 가능
 - 각 서비스 별로 인증/인가를 구현해야하며, 공통 로직(인증/인가 등)이 각 서비스마다 존재하게 됨
 - 전체 API 별 통계나 API 별 과금이 힘듬

API Gateway는 사용자가 서비스를 이용하기 위해 모든 엔드포인트를 알 필요가 없이 API Gateway만 알고 있으면, API Gateway가 알아서 각각의 서비스로 라우팅을 해주어 사용자가 서비스를 받을 수 있습니다. 그리고 인증/인가와 같은 공통 로직 처리를 API Gateway에서 처리할 수 있고, API 별 통계나 과금 처리를 API Gateway에서 처리할 수 있습니다. 




비단, API Gateway는 MSA 구조에서만 고려대상이 아니라, 기존 Monolithic Architechture 구조에서도 서비스가 많은 경우 사용 될 수도 있을 것 같습니다.


 
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTU3MTI2ODM2NiwtOTE2MTI5OTM0LDE3OD
k1MjcyMjddfQ==
-->