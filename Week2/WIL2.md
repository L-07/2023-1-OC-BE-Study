# Week2 WIL

## 회원 관리 예제 - 백엔드 개발

### 비즈니스 요구사항 정리
> 일반적인 웹 애플리케이션 계층 구조는 다음과 같다. ![웹 애플리케이션 계층 구조](https://user-images.githubusercontent.com/80233303/228443326-80296160-67b2-4351-9abb-5be30af7d75a.jpeg)  
> *Ref. 스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술 강의 자료(김영한)*  
> \
> **컨트롤러** : Presentation Layer에 속한 클래스로, 웹 MVC의 컨트롤러 역할을 해 외부 요청을 받아 서비스 레이어로 넘긴다.  
> **서비스** : Service Layer 혹은 Business Layer로 애플리케이션 비즈니스의 핵심 로직을 구현한다.  
> **리포지토리** : Repository Layer 혹은 Data Access Layer로 데이터베이스에 접근하고 도메인 객체를 데이터베이스에 저장하고 관리한다.  
> **도메인** : 비즈니스 도메인 객체로, 영역을 나누듯 도메인을 나누고 또 하위 도메인을 나눌 수 있다.  
> \
> ![클래스 의존 관계](https://user-images.githubusercontent.com/80233303/229361676-83dde669-bd85-4c02-9dc9-28d5ee986af4.png)  
> *Ref. 스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술 강의 자료(김영한)*  
> \
> 클래스 의존 관계 : 회원 서비스가 있는 회원 비즈니스 로직이 회원 리포지토리 인터페이스에 클래스로 상속되고 메모리 회원 리포지토리는 회원 리포지토리에 인터페이스로 상속된다.

### 회원 도메인과 리포지토리 만들기
리포지토리 : 회원 도메인 객체를 저장하고 불러올 수 있는 것
> src/main/java/repository에 MemberRepository 인터페이스를 생성한다.
> ```java
>   package hello.hellospring.repository;
>
>   import hello.hellospring.domain.Member;
>
>   import java.util.List;
>   import java.util.Optional;
>
>   public interface MemberRepository {
>       Member save(Member member);
>       Optional<Member> findById(Long id);
>       Optional<Member> findByName(String name);
>       List<Member> findAll();
>}
>```
> 위와 같이 회원 저장, id로 검색, 이름으로 검색, 전체 검색 네 가지의 리포지토리 기능을 만들 수 있다. 이때 Optional은 기존의 NPE(NullPointerExceptioin)을 방지하기 위해 JAVA8부터 추가된 클래스이다.  
> \
> src/main/java/repository에 MemoryMemberRepository 클래스를 만든다.
> ```java
>   package hello.hellospring.repository;
>
>   import hello.hellospring.domain.Member;
>
>   import java.util.*;
>
>   public class MemoryMemberRepository implements MemberRepository {
>
>       private static Map<Long, Member> store = new HashMap<>();
>       private static long sequence = 0L;
>
>   @Override
>   public Member save(Member member) {
>       member.setId(++sequence);
>       store.put(member.getId(), member);
>       return member;
>   }
>
>   @Override
>   public Optional<Member> findById(Long id) {
>       return Optional.ofNullable(store.get(id));
>   }
>
>   @Override
>   public Optional<Member> findByName(String name) {
>       return store.values().stream()
>               .filter(member -> member.getName().equals(name))
>               .findAny();
>   }
>
>   @Override
>   public List<Member> findAll() {
>       return new ArrayList<>(store.values());
>   }
>
>   public void clearStore() {
>       store.clear();
>    }
> ```
> Map으로 long과 회원을 저장하고 sequence로 회원이 생성될 때 마다 값을 올려준다.

### 회원 서비스 개발
> src/main/java/service에 MemberService클래스를 생성한다.
> ```java
>   package hello.hellospring.service;
>
>   import hello.hellospring.domain.Member;
>   import hello.hellospring.repository.MemberRepository;
>   import hello.hellospring.repository.MemoryMemberRepository;
>
>   import java.util.List;
>   import java.util.Optional;
>
>   public class MemberService {
>
>   private final MemberRepository memberRepository;
>
>   public MemberService(MemberRepository memberRepository) {
>       this.memberRepository = memberRepository;
>   }
>
>   // 회원 가입
>   public long join(Member member) {
>       // 같은 이름이 있는 중복 회원X
>       validateDuplicateMember(member); // 중복 회원 검증
>       memberRepository.save(member);
>       return member.getId();
>   }
>
>   private void validateDuplicateMember(Member member) {
>       memberRepository.findByName(member.getName())
>               .ifPresent(m -> {
>                   throw new IllegalStateException("이미 존재하는 회원입니다.");
>               });
>   }
>
>   // 전체 회원 조회
>   public List<Member> findMembers() {
>       return memberRepository.findAll();
>   }
>
>   public Optional<Member> findOne(Long memberId) {
>       return memberRepository.findById(memberId);
>   }
>
> }
> ```
> memberRepository.save(member)를 통해 회원을 저장하고 Id를 반환한다.  
> 비즈니스 로직으로 동명의 회원을 방지하는 기능이 존재한다면 findByName을 통해 중복 회원을 검증한다.

> TDD란 test-driven development로 **테스트 주도 개발**을 뜻한다.  
> 프로그램 개발 시 확장가능성이 요구되고, 규모가 커질수록 설계에 요하는 시간과 비용이 커지며, 계획대로 흘러가지 않을 수 있다는 문제 등이 있다. 이러한 문제점들로 인해 등장한 개발방법론이 TDD로, 테스트 케이스를 먼저 작성한 후 테스트 케이스어 맞춰 실제 개발 단계로 이행하는 방법이다.  
> TDD는 다음과 같은 단계로 이루어진다.
> 1. 테스트 케이스 작성
> 2. 테스트 케이스 실행
> 3. 테스트 케이스를 통과할 수 있는 가장 단순한 코드 작성
> 4. 모든 테스트 통과
> 5. 필요에 따라 리팩터링(테스트 결과에 변화 없이 코드의 구조 재조정)
> 
> 새로운 기능을 추가할 때마다 위와 같은 과정을 반복하여 코드를 작성한다.  
> 이때 새로운 기능은 클래스 등의 작은 단위로 유지하여야 가독성도 높고 디버깅에도 큰 어려움을 겪지 않을 수 있다.  
> \
> TDD는 위와 같은 과정을 통해 코드의 유지보수가 용이해지고 프로그래밍 시간이 단축된다는 장점이 있다.