---
layout: single
title: "1장 오브젝트와 의존관계 - 1.1 초난감 DAO"
categories:
  - "토비의 스프링 3.1"
tags:
  - "오브젝트와 의존관계"
---

> '토비의 스프링 3.1' 은 스프링 개발자만이 아닌 모든 개발자라면 반드시 읽어야 하는 프로그래밍 원리를 담고 있는 책이다.

스프링이 지원하는 세가지 핵심 프로그래밍 모델

1. IoC/DI 라고 불리는 오브젝트의 생명주기와 의존관계에 대한 프로그래밍 모델
2. 서비스의 추상화
3. AOP 라는 애플리케이션 코드에 산재해서 나타나는 부가적인 기능을 독립적으로 모듈화하는 프로그래밍 모델

스프링을 사용한다는 것은 이 세가지 요소를 적극적으로 활용한다는 것이다.

## 1.1 초난감 DAO

스프링은 오브젝트에 관심을 둔다.

오브젝트에 대한 관심은 오브젝트 설계 방법인 디자인 패턴, 구조를 개선하는 리팩토링, 검증하는데 쓰이는 테스트를 발전시켰다.

### 1.1.1 User

사용자 정보를 저장하기 위해 자바빈 규약을 따르는 User 클래스를 만든다.

```java
public class User {

  String id;
  String name;
  String password;

  public String getId {
    return id;
  }
  public void setId(String id) {
    this.id = id;
  }
  public String getName() {
    return name;
  }
  public void setName(String name) {
    this.name = name;
  )
  public String getPassword() {
    return password;
  }
  public void setPassword(String password) {
    this.password = password;
  }

}
```

<div class="notice--primary" markdown="1">
자바빈(JavaBean)은 두 가지 관례를 따라 만들어진 오브젝트를 가리킨다.<br>
디폴트 생성자 : 프레임워크에서 리플렉션을 이용해 오브젝트를 생성하기 위해 필요하다.<br> 
프로퍼티 : 자바빈이 노출하는 이름을 가진 속성을 프로퍼티라고 한다. 프로퍼티는 set, get 메소드를 이용해 수정, 조회할 수 있다.<br>
<br>
1장_ 오브젝트와 의존관계, 55.<br>
</div>

### 1.1.2 UserDao

UserDao 는 User 객체를 DB 에 저장하고 불러올 수 있는 클래스이다.

DB 를 사용하기 위해서는 JDBC 를 이용해야 된다.

이를 위해 UserDao 를 구현해 본다.

구현된 UserDao 는 User 데이터를 DB 에 저장하는 add 메소드와 저장된 User 데이터를 가져오는 get 메소드를 가지고 있다.

```java
public class UserDao {

  public void add(User user) throws ClassNotFoundException, SQLException {
    // DB 연결을 위한 커넥션을 가져온다.
    Class.forName("com.mysql.jdbc.Driver");
    Connection c = DriverManager.getConnection("jdbc:mysql://localhost/springbook?characterEncoding=UTF-8", "spring", "book");

    // SQL 을 담은 statement 를 만든다.
    PreparedStatement ps = c.prepareStatement("insert into users(id, name, password) values(?,?,?)");
    ps.setString(1, user.getId());
    ps.setString(2, user.getName());
    ps.setString(3, user.getPassword());

    // 데이터를 DB 에 추가한다.
    ps.executeUpdate();

    // resultSet, prepareStatement, connection 을 반환한다.
    ps.close();
    c.close();
  }

  public User get(String id) throws ClassNotFoundException, SQLException {
    // DB 연결을 위한 커넥션을 가져온다.
    Class.forName("com.mysql.jdbc.Driver");
    Connection c = DriverManager.getConnection("jdbc:mysql://localhost/springbook?characterEncoding=UTF-8", "spring", "book");

    // SQL 을 담은 statement 를 만든다.
    PreparedStatement ps = c.prepareStatement("select * from users where id = ?");
    ps.setString(1, id);

    // 데이터를 DB 에서 가져온다.
    ResultSet rs = ps.executeQuery();
    rs.next();

     // 가져온 데이터를 User객체에 담는다.
    User user = new User();
    user.setId(rs.getString("id"));
    user.setName(rs.getString("name"));
    user.setPassword(rs.getString("password"));

    // resultSet, prepareStatement, connection 을 반환한다.
    rs.close();
    ps.close();
    c.close();

    return user;

  }

}
```

### 1.1.3 main() 을 이용한 DAO 테스트 코드

만들어진 코드의 기능을 검증하기 위해 UserDao 에 main() 메소드를 만들어서 UserDao 를 테스트 해본다.

```java
public class UserDao {

  // ...

  public static void main(String[] args) throws ClassNotFoundException, SQLException {
    UserDao dao = new UserDao();

    User user = new User();
    user.setId("whiteship");
    user.setName("백기선");
    user.setPassword("married");

    dao.add(user);

    System.out.println(user.getId() + " 등록 성공");

    User user2 = dao.get(user.getId());
    System.out.println(user2.getName());
    System.out.println(user2.getPassword());

    System.out.println(user2.getId() + " 조회 성공");
  }

}
```

테스트용 main 메소드를 실행해보면 잘 작동하는 것을 확인할 수 있다.

위의 UserDao 는 기능과 테스트는 돌아가지만 안타깝게도 객체지향적으로 만들어진 코드는 아니다.

이제부터 이 UserDao 를 고쳐가면서 객체지향 기술의 원리에 충실한 코드를 만들어 보겠다.
