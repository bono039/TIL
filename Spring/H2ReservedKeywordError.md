## 에러
Spring 프로젝트에서 <code>user</code> 테이블에 쿼리문을 작성하고 SpringBoot 애플리케이션을 실행했는데 에러가 났다. <br/>

![image](https://github.com/user-attachments/assets/ec606857-b78a-49c3-b513-cde2f1272923)

<br/>

<br/>

## 발생 이유
<code>user</code>는 H2에서 예약된 키워드이기 때문에 테이블명으로 사용할 수 없다.

<br/>

## 해결 방법
1. user 테이블을 사용하고 싶으면, **구분자**를 이용해 구분한다.
```
@Entity  
@Table(name = "\"user\"")
```

<br/>

2. 예약어가 아닌 단어로 테이블명을 변경한다. (예: user → users)
![image](https://github.com/user-attachments/assets/c72940f4-a4d0-453f-939d-a6045040a735)

<br/>

![image](https://github.com/user-attachments/assets/6f66e908-8e03-4e47-b7f0-7922eb5d95f7)


<br/>

## 결과
POSTMAN으로 HTTP 요청해 클라이언트에서 확인해보면, 에러 없이 해당 테이블에 원하던 쿼리문이 잘 적용된 것을 볼 수 있다.

![image](https://github.com/user-attachments/assets/07bf26ca-c2b0-433b-9424-463633379117)

<br/>

<br/>

### 🔗 참고
* https://stackoverflow.com/questions/72178354/spring-sql-org-h2-jdbc-jdbcsqlsyntaxerrorexception-syntax-error-in-sql-stateme%8A%94-%EB%AC%B8%EC%A0%9C)
* [H2 예약어들](https://www.h2database.com/html/advanced.html#keywords)
