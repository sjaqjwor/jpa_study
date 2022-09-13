### JPA가 복잡한 쿼리를 지원하는 방법

- Jpa를 사용하면 엔티티 객체를 중심으로 개발
- jpql , criteria , queryDsl , native 등등
- 해당 JPA에서 제공하는 것들은 쿼리 질의시 flush발생

### JPQL

- SQL을 추상화해서 객체 지향 쿼리 언어를 제공
- 몇가지 컬럼이 아닌 엔티티 객체를 대상으로 쿼리
- 특정 데이터베이스에 의존x

~~~
select m from Memeber m
~~~

- jpql도 작성을 해도 결국 해당 jpql을 대상으로 이에 맞는 쿼리를 생성해서 쿼리를 실행
- 컴파일 시점에 오류 못차고 해당 쿼리직접쓸때 발견 가능

#### 기본 문법

- 엔티티 속성은 대소문자 구분 하나 JPQL 키워드는 대소문자 구분x

- 별칭은 필수다 

- 파라미터 바인딩 당연가능(where)

- 반환은 typedQuery , Query가 존재

  - typedQuery 정확한 반환타입이 존재

    - typedQuery<Entity>

      ~~~
      select m from Memeber m , Member.class
      ~~~

      

  - Query정확한 반환타입 x

    ~~~
    select m.name from Memeber m
    ~~~

    - 이렇게 엔티티 전체가 아니라 원하는 컬럼만 가지고 오려고 할때
    - 가져오는 방법은 Object[] , new DTO로 반환 반을수 있다

- 프로젝션은 select절에 조회할 대상을 지정(엔티티 , 임베디드 타입 , 숫자 , 문장 등등)

  - 프로젝션으로 엔티티를 가져오면 영속성 컨텍스트에 관리된다!!!

- 페이징도 가능(firestResult , MaxResult)

- join도 당근 가능

  ~~~
  select ~ from Member m join m.tean t
  select ~ from Member m left join m.team
  select _ from Member m left join m.team t on t.name = "A" 
  ~~~

  - 조인 대상 필터링 가능 (on 키워드 사용) 그러나 버전에 따라 지원 가능여부

- 서브 쿼리는 지원하나 .. from 절 서브쿼리는 지원하지 않는다.

  - jpa스펙은 where , having 만 가능하다 하이버네이트에서는 select도 지원

- jpql에는 기본적으로 제공하는 함수가 있다

  - 사용자 정의 함수도 가능함..
  - 쓰려면 디비에 맞는 다이얼럭트를 상속 받은 커스텀한 다이얼럭트를 생성 후 function으로 등록 하면 된다

### Criteria

- 빌더 역할

- jpql은 동적 쿼리를 생성하기 어렵다
  - if문 ~~~ 
    - criteria도 마찬가지지만 문자열로 하는게 아니기에 jpql보다는 좋다
  - 띄어쓰기도 중요..
- 다양한 메소드 제공
- 사용해본결과 코드가 너무 길어지는 문제도 있고 , 뭔지 모르게 복잡.. 
  - 영한님도 안쓴다고... 저도 안쓰고 싶습니다.

### QueryDsl

- Criteria처럼 빌드 역할
- Criteria에 비해 더 SQL에 가까워서 빌드를 할때에도 보기 쉽다.
- 컴파일 시점에 오류 찾기 쉬움

### Native SQL

- 직접 생쿼리 작성
- JPQL에서 제공 못하는 쿼리를 할 수 있게 제공하는 역할