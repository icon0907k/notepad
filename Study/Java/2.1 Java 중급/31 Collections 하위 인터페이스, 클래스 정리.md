### 인터페이스
자바 컬렉션 프레임워크의 핵심 인터페이스는 다음과 같이 구성되어 있다.

- **List** : 순서가 있는 컬렉션으로, 중복 요소를 허용한다. 요소는 인덱스를 통해 접근할 수 있다.  
    예 : `ArrayList`, `LinkedList`
    
- **Set** : 중복 요소를 허용하지 않는 컬렉션을 나타내며 특정 위치가 없기 때문에 인덱스를 통한 접근이 불가능하다.  
    예 : `HashSet`, `LinkedHashSet`, `TreeSet`
    
- **Queue**:  요소가 처리되기 전에 저장되는 컬렉션을 나타낸다.  
    예 : `ArrayDeque`, `LinkedList`, `PriorityQueue`
    
- **Map** : 키와 값 쌍으로 요소를 저장하는 객체이다. `Map` 인터페이스는 `Collection` 인터페이스를 상속받지 않습니다.  
    예 : `HashMap`, `LinkedHashMap`, `TreeMap`
    
- **Collection** : 모든 컬렉션 클래스가 상속받는 단일 루트 인터페이스입니다. 이 인터페이스는 `List`, `Set`, `Queue`와 같은 여러 하위 인터페이스를 포함한다.
    

### 구현
자바는 각 인터페이스에 대해 다양한 구현을 제공한다

- **List** : `ArrayList`는 내부적으로 배열을 사용하고 `LinkedList`는 연결 리스트를 활용한다.
    
- **Set** : `HashSet`은 해시 테이블을, `LinkedHashSet`은 해시 테이블과 연결 리스트를, `TreeSet`은 이진 검색 트리를 사용한다.
    
- **Map** : `HashMap`은 해시 테이블을 기반으로 하고, `LinkedHashMap`은 해시 테이블과 연결 리스트를, `TreeMap`은 이진 검색 트리를 사용한다.
    
- **Queue** : `LinkedList`는 연결 리스트를 사용하고, `ArrayDeque`는 배열 기반의 원형 큐를 사용한다. 일반적으로 `ArrayDeque`가 더 빠르다.
    

### 선택 가이드
- **순서가 중요하고 중복이 허용되는 경우** : `List` 인터페이스를 선택하자. 일반적으로 `ArrayList`가 선호되지만, 추가/삭제 작업이 자주 발생하는 경우 `LinkedList`가 성능상 더 나은 선택이 될 수 있다.
    
- **중복을 허용하지 않고 순서가 중요하지 않은 경우** : `HashSet`을 사용하자. 순서를 유지해야 한다면 `LinkedHashSet`, 정렬된 순서가 필요하면 `TreeSet`을 선택하자.
    
- **요소를 키-값 쌍으로 저장하려는 경우** : `Map` 인터페이스를 선택하자. 순서가 중요하지 않다면 `HashMap`, 순서를 유지해야 한다면 `LinkedHashMap`, 정렬된 순서가 필요하다면 `TreeMap`을 사용하자.
    
- **요소를 처리하기 전에 보관해야 하는 경우** : `Queue` 또는 `Deque` 인터페이스를 사용하자. 스택이나 큐 구조 모두 `ArrayDeque`가 가장 빠르며 우선순위에 따라 요소를 처리해야 한다면 `PriorityQueue`를 고려하자.
    

### 실무 선택 가이드

- 대부분의 경우 `List`는 `ArrayList`를 사용한다.
- `Set`의 경우 일반적으로 `HashSet`을 사용한다.
- `Map`의 경우 대개 `HashMap`을 선택한다.
- `Queue`에서는 대부분 `ArrayDeque`를 사용한다.

