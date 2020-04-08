<a href='https://futurecreator.github.io/2018/10/18/service-discovery-in-microservices/'>source</a>

# 서비스 디스커버리
클라우드에서 인스턴스는 동적으로 할당되기 때문에 IP주소나 포트 정보가 정해지지 않은 데다가 오토스케일링도 일어나고 중지되고 복구되면서 네트워크 위치가 계속해서 바뀌게 됩니다.

따라서 클라이언트나 API 게이트웨이가 호출할 서비스를 찾는 매커니즘이 필요하고 이를 서비스 디스커버리(Service Discovery)라고 합니다.

https://www.nginx.com/blog/service-discovery-in-a-microservices-architecture/

## 클라이언트 사이드 디스커버리
서비스 인스턴스의 네트워크 위치를 찾고 로드밸런싱하는 역할을 클라이언트가 담당하는 방식입니다.
https://www.nginx.com/blog/service-discovery-in-a-microservices-architecture/

이러한 방식의 장점은 서비스 디스커버리 로직을 클라이언트가 가지고 있기 때문에 서비스에 맞는 로드밸런싱 방식을 각자 구현할 수 있다는 점입니다. 하지만 반대로 각 서비스마다 서비스 레지스트리를 구현해야 하는 종속성이 생깁니다. 만약 서비스마다 다른 언어를 사용하고 있다면 언어별 또는 프레임워크별로 구현해야겠죠.

## 서버 사이드 디스커버리
https://www.nginx.com/blog/service-discovery-in-a-microservices-architecture/
클라이언트는 로드밸런서로 요청을 보냅니다. 로드밸런서는 서비스 레지스트리를 조회해서 가용한 인스턴스를 찾고 그 중 선택해서 요청을 라우팅하는 방식입니다. 서비스 레지스트리에 등록되는 방식은 클라이언트에 있을 때와 같습니다.

## 서비스 레지스트리
서비스 레지스트리는 각 서비스 인스턴스의 네트워크 위치 정보를 저장하는 데이터베이스로 항상 최신 정보를 유지해야 하며 고가용성이 필요합니다.

앞서 얘기한 서비스 레지스트리인 Netflix Eureka 는 서비스 인스턴스를 등록하고 조회하는 API를 제공합니다. 

### 참고 자료
[ Service Discovery in a Microservices Architecture | NGINX Blog](https://www.nginx.com/blog/service-discovery-in-a-microservices-architecture/)