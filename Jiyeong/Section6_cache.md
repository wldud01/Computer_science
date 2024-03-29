## 캐시 메모리

주기억 장치와 관련된 내용은 아니지만 중요한 내용이다. 

CPU가 메모리에 접근하는 시간은 CPU 연산 속도보다 느리다. 

### 저장 장체 계층 구조

캐시 메모리를 이해하기 위해서는 저장 장치 계층 구조를 이해할 필요가 있다. 

- CPU와 가까운 저장 장치는 빠르고 멀리 있는 저장 장치는 느리다.
- 속도가 빠른 저장 장치는 저장 용량이 작고 가격이 비싸다.

CPU와 가장 가까운 레지스터는 일반적으로 램 보다 용량은 작지만 접근 시간이 세가지 중에서 가장 빠르가 가격도 비싸다. USB 메모리보다 접근 시간이 가까운 램은 당연히 접근 시간은 훨씬 빠르지만 같은 용량이라도 더 비싸다.

즉 낮은 가격대의 대용량 저장 장치를 원한다면 느린 속도는 감수해야 하고 빠른 속도의 저장 장치를 원한다면 작은 용량과 비싼 가격을 감수해야한다.

메모리 계층 구조라고도 하고 여기서 메모리는 램이 아니라 일반적인 저장장치를 의미한다.

### 캐시 메모리

CPU와 메모리 사이에 위치한 레지스터보다는 용량이 크고 메모리보다 빠른 SRAM 기반의 저장 장치이다.

CPU의 연산 속도와 메모리 접근 속도의 차이를 조금이라도 줄이기 위해서 탄생했다. 매번 왔다 갔다하는 것은 오래 걸리기 때문에 메모리에서 CPU가 사용할 일부 데이터를 미리 캐시 메모리로 가져와서 쓰는 것이다.

캐시메모리는 CPU가 자주 사용할 법한 내용을 미리 저장해서 사용한다. CPU 내부에 있는 캐시 메모리도 있기는 있지만 레지스터가 더 빠르다.

### 계층적 캐시 메모리

- L1
    
    레지스터보다 용량은 크지만 L2보다 작고 가장 빠르다. CPU와 가장 가까운 위치이기 때문이다.
    
- L2
    
    L3보다는 작지만 접근 속도는 빠르다.  L1과 마찬가지로 코어 내부에 위치해 있다
    
- L3
    
    캐시 메모리 중에서 상대적으로 느리고 코어 외부에 있다. 메모리 보다는 용량이 작지만 캐시 메모리 중에서 용량이 가장 크다.
    

### 멀티 코어 프로세서의 캐시 메모리

멀티 코어 프로세서같은 경우에는 CPU 각각이 캐시를 공유해서 쓰기도 한다. 멀티 코어 프로세서의 캐시 메모리 같은 경우에는 코어 마다 L1 L2 캐시를 따로 두기 때문에 각각의 코어에 있는 이 캐시들이 다른 내용을 저장할 수도 있다. 그러면 데이터 일관성이 깨질 수 있기 때문에 싱크(동기화)하는 작업이 매우 중요한 요소가 된다.

L1 캐시는 명령어만을 담거나 데이터만을 담고 있는 캐시 메모리를 따로 만들어서 관리하기도 한다. 이런 것은 분리형 캐시 메모리라고 한다.

### 참조 지역성의 원리

locality of refference

캐시 메모리의 모든 내용을 저장할 수는 없다는 점을 고려해서 살펴보아야 한다. 캐시메모리는 CPU가 자주 사용할 법한 내용을 예측을 해서 저장한다. 예측이 맞았을 경우(CPU가 정말 사용하는 경우)를 캐시 히트라고 한다. 예측이 틀렸을 경우 캐스 미스라고 한다. 캐시 미스는 CPU가 직접 메모리에 접근해야 하기 때문에 성능이 떨어진다고 할 수 있다.

- 캐시 적중률
    
    캐시 히트 횟수 / (캐시 히트 횟수 + 캐시 미스 횟수)
    
    캐시 적중률을 높여하는 것이 관건이다.
    

참조 지역성의 원리는 CPU가 메모리에 접근할 때의 주된 경향을 바탕으로 만들어진 원리이다.

- CPU는 최근에 접근했던 메모리 공간에 다시 접근하려는 경향이 있다
- CPU는 접근한 메모리 공간 근처를 접근하려는 경향이 있다.

이 두가지 경향성을 바탕으로 사용할 법한 대상을 예측해서 저장한다.

접근했던 메모리 공간에 다시 접근하려는 경향이라는 것은 구구단 코드를 작성했을 때 num이라는 숫자에 2가 저정되어서 2단을 출력한다고 한다면 num이 여러 번 다시 참조된다. 

메모리 공간 근처를 접근하려는 경향이 있다는 것은 보통 프로그램은 관련된 데이터가 모여있는 경우가 많은데 이러한 모여있는 데이터에 집중적으로 접근하려고 할 것이다.
