---
layout: single
title: 클린코드 스터디 - 009
categories: 
  - bookstudy - cleancode
tags: 
  - cleancode
---

## 8장 단위테스트

### TDD 법칙 세 가지

첫째 법칙: 실패하는 단위 테스트를 작성할 때까지 실제 코드를 작성하지 않는다.

둘째 법칙: 컴파일은 실패하지 않으면서 실행이 실패하는 정도로만 단위 테스트를 작성한다.

셋째 법칙: 현재 실패하는 테스트를 통과할 정도로만 실제 코드를 작성한다.

위 세가지 규칙을 따르면 개발과 테스트가 대략 30초 주기로 묶인다. 

테스트 코드와 실제코드가 함께 나올뿐더러 테스트 코드가 실제코드보다 불과 몇초전에 나온다.


### 테스트코드 용어 개념

* 테스트 케이스: 테스트 코드 하나 하나를 의미
* 테스트 유닛: 테스트 케이스의 집합
* 테스트 슈트: 테스트 유닛의 집합
* 즉, 케이스 < 유닛 < 슈트

### 깨끗한 테스트 코드 유지하기

테스트 코드는 실제코드 못지 않게 중요하다. 테스트 코드는 사고와 설계와 주의가 필요하다. 실제코드 못지 않게 깨끗하게 짜야 한다.

테스트는 유연성, 유지보수성, 재사용성을 제공한다.

코드에 유연성, 유지보수성, 재사용성을 제공하는 버팀목이 바로 단위테스트다. 

아키텍처가 아무리 유연하더라도, 설계를 아무리 잘 나눴더라도, 테스트 케이스가 없으면 개발자는 변경을 주저한다. 버그가 숨어들까 두렵기 때문이다.

하지만, 테스트 케이스가 있다면 공포는 사실상 사라진다. 테스트 커버리지가 높을수록 공포는 줄어든다. 아키텍처가 부실한 코드나 설계가 모호하고 엉망인 코드라도 별다른 우려 없이 변경할 수 있다.
오히려 안심하고 아키텍처와 설계를 개선할 수 있다.

그러므로 실제 코드를 점검하는 자동화된 단위 테스트 슈트는 설계와 아키텍처를 최대한 깨끗하게 보존하는 열쇠다. 테스트는 유연성, 유지보수성, 재사용성을 제공한다.
테스트케이스가 있으면 변경이 쉬워지기 때문이다. 

결론, 테스트코드가 지저분할수록 실제코드도 지저분해진다.



### 테스트 당 assert 하나

Unit	으로 테스트 코드를 짤 때는 함수마다 assert 문을 단 하나만 사용해야 한다고 주장하는 학파가 있다. 가혹한 규칙이라 여길지도 모르지만, 목록 9-5를 보면 , 확실히 쉽고 빠르다.
하지만 목록 9-2는 어떨까? 목록 9-2는 “출력이 XML이다.”라는 assert 문과 “특정 문자열을 포함한다.”는 assert문을 하나로 병합하는 방식이 불합리해 보인다. 하지만 목록9-7에서 보듯이 테스트를 두개로 쪼개 각자가 assert를 수행하면 된다.





given-when-then이라는 관례를 사용해서 테스트 코드를 더 읽기 쉽게 만든 것을 볼 수 있다.

하지만 위와 같이 1개의 assert문을 주도록 강제할 경우에는 중복되는 코드가 많아질 위험이 있습니다.

TEMPLATE METHOD 패턴을 사용하여 해결 할 수 있다.
given/when 부분을 부모 클래스에 두고
then 부분을 자식 클래스에 두는 방식으로
하지만 공통 부분을 별도의 method로 끄집어 내는데 들어가는 노력이 더 클 수 있습니다.

(given, when 의 공통부분은 부모 클래스에서 정의하고, 서로 다른 then 부분은 각자 자식 클래스를 만들어 구현하면 중복을 잡을 수 있다.)


결국, 필요한 경우에는 assert를 여러개를 두되 assert 문 개수는 최대한 줄이려고 노력하는 것이 좋습니다.

### 테스트 당 개념 하나

위의 테스트 당 assert 하나 보다는 테스트 함수마다 한 개념만 테스트하라라는 규칙이 더 낫습니다.
* 잡다한 개념을 연속으로 테스트하는 것은 피함
아래 코드는 3가지 개념을 한 테스트 함수에서 테스트하는 예시입니다.
* 이러한 경우 각 절이 존재하는 이유와 테스트 하는 개념을 다 이해해야 해서 좋지 않음



결국 테스트 함수와 assert문에 관련된 규칙은 아래 2개가 가장 좋다고 하겠다.
1. 개념 당 assert문 수를 최소로 줄여라
2. 테스트 함수 하나는 개념 하나만 


### F.I.R.S.T
깨끗한 테스트는 다음 다섯 가지 규칙을 따르는데, 각 규칙에서 첫 글자를 따오면 FIRST 가 된다.
1. 빠르게^Fast : 테스트는 빨라야 한다.
    * 테스트는 빨리 돌아야 한다.
    * 테스트가 느리면 자주 돌릴 엄두를 못낸다
    * 자주 돌리지 않으면 초반에 문제를 찾아내 고치지 못한다.
    * 코드를 마음껏 정리하지도 못한다.
    * 결국 코드 품질이 망가지기 시작한다.

2. 독립적으로^Independent : 각 테스트는 서로 의존하면 안 된다.
    * 한 테스트가 다음 테스트가 실행될 환경을 준비해서는 안된다.
    * 각 테스트는 독립적으로 그리고 어떤 순서로 실행해도 괜찮아야 한다.
    * 테스트가 서로에게 의존하면 하나가 실패할 때 나머지도 잇달아 실패하므로 원인을 진단하기 어려워지며 후반 테스트가 찾아내야 할 결함이 숨겨진다.

3. 반복가능하게^Repeatable : 테스트는 어떤 환경에서도 반복 가능해야 한다.
    * 실제 환경, QA 환경, 버스를 타고 집으로 가는 길에 사용하는 노트북 환경에서도 실행할 수 있어야 한다.
    * 테스트가 돌아가지 않는 환경이 하나라도 있다면 테스트가 실패한 이유를 둘러댈 변명이 생긴다.
    * 환경이 지원되지 않기에 테스트를 수행하지 못하는 상황에 직면할 수 있다.

4. 자가검증하는^Self-Validating : 테스트는 Bool 값으로 결과를 내야 한다. 성공 아니면 실패다.
    * 통과 여부를 알려고 로그 파일을 읽게 만들어서는 안된다.
    * 통과 여부를 보려고 텍스트 파일 두 개를 수작업으로 비교하게 만들어서도 안 된다.
    * 테스트가 스스로 성공과 실패를 가늠하지 않는다면 판단은 주관적이 되며 지루한 수작업 평가가 필요하게 된다.

5. 적시에^Timely : 테스트는 적시에 작성해야 한다.
    * 단위 테스트는 테스트하려는 실제 코드를 구현하기 직전에 구현한다.
    * 실제 코드를 구현한 다음에 테스트 코드를 만들면 실제 코드가 테스트하기 어렵다는 사실을 발견할지도 모른다.
    * 어떤 실제 코드는 테스트하기 너무 어렵다고 판명날지도 모른다.
    * 테스트가 불가능하도록 실제 코드를 설계할지도 모른다.


## 참고
- 
