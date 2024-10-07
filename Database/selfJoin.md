# 셀프 조인

## 정의
1개의 테이블(x)에 가상으로 별칭을 부여해 2개의 테이블인 것처럼 간주한 후 JOIN하는 것

<br/>

## 예
1. 위계성 데이터를 다룰 때
2. 순차성 데이터를 다룰 때
3. 한 테이블 안에 관계성이 명시되어야 할 데이터가 여러 개 존재할 때

<br/>

## 표현식
```
select x1.컬럼이름A, 
       x1.컬럼이름B,
       x2.컬럼이름C, ...
from 테이블이름X x1, 테이블이름 X x2
where x1.컬럼이름A=X2.컬럼이름C;
```

<br/>

### 유의사항
**_ALIAS 지정 필수_** = 테이블명을 꼭 지정해 줘야 한다.

<br/>

### 참고
- https://m.blog.naver.com/regenesis90/222190494489
- https://minor-research.tistory.com/24
- https://kimsyoung.tistory.com/entry/SELF-JOIN-%E4%B8%8B-%EC%85%80%ED%94%84-%EC%A1%B0%EC%9D%B8%EC%9D%98-%EC%9A%A9%EB%A1%80
