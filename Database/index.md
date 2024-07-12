# 인덱싱
> 정의
> 
> 특징
> 
> 실행 이유
> 
> 종류
>
> 사용 지침

<br/>

## 정의
> 테이블에 대한 데이터 검색 속도를 높이고자 생성하며 컬럼에 적용
- <code>인덱스-데이터 레코드</code>쌍으로 구성
![image](https://github.com/bono039/TIL/assets/67899934/510f4df6-0538-47de-b6f2-94786beb9ffb)
*출처 : 데이터베이스 배움터

<br/>

## 특징
- 정렬되어 있다.
- 실제 데이터(레코드)는 정렬이 되어 있을 수도 안 되어 있을 수도 있다. ⇒ 클러스터드/넌클러스터드 인덱스
- 공간 차지
- 한 릴레이션 당 3개 이하의 인덱스 만들어야 함 (CRUD가 일어나면 인덱스를 많이 업데이트 해야 하므로)
- **삽입,수정,삭제가 자주 발생하지 않는 경우** 활용
- Integer형 속성에 인덱스를 만드는 것이 가장 좋고, 고정 길이 속성에 인덱스를 만드는 것이 좋다.
- 대량 데이터 삽입 시, 모든 인덱스를 제거하고 데이터 삽입이 끝난 후 인덱스들을 다시 생성하는 것이 좋음

<br/>

## 실행 이유
- 테이블에 **없는 정보 검색 시 빠른 판단 가능**

<br/>

## 장단점
### 장점
- 인덱싱에 따른 오버헤드 발생
   * 인덱스를 위한 메모리 추가 소모
   * 데이터 수정,삭제 시 인덱스까지 수정해야 함

<br/>

## 종류
### B-TREE 인덱스
#### 구성
- Root 블록 : 분기 값 저장, 인덱스 다음 단계의 브랜치 블록을 가리키는 항목 포함
- Branch 블록 : 분기 값 저장, <code><Separator Key, DBA></code>로 구성, 아래 블록 가리킴
- Leaf 블록 : 인덱스 키 값 + ROWID 저장. <code><실제 Key값, ROWID></code>로 구성

#### 특징
- SELECT, INSERT, DELETE 등 작업에 효율적
- 분포도가 낮은 컬럼에 불리. OR 연산자에 대해 테이블 전체를 FULL SCAN하는 것은 위험
- 테이블의 어느 데이터에 접근하더라도 동일한 성능 보장
- <code>Double Linked List</code> : Leaf 블록 간 양방향 통신 의미. Leaf 블록의 한 인덱스 값에서 Range 스캔을항

<br/>

#### 고려사항
- 랜덤 액세스 양을 신경써야 함 (지나치게 많아선 안 됨)
- 처리 범위를 최소화 해 렌덤 액세스를 감소시키는 것이 중요 (AND 조건 : 처리 범위 감소, OR 조건 : 처리 범위 증가시킴)

<br/>

### 비트맵 인덱스
<code>
  CREATE BITMAP INDEX 인덱스명 ON 테이블명(컬럼)
  [TABLESPACE 테이블 스페이스명]
</code>
  
- 비트맵(0과 1)로 인덱스 관리
- 비트 이용해 컬럼값을 저장하고, ROWID 자동 생성
- 분포도 낮은 컬럼에 적용 시 유용 (= 컬럼이 갖는 데이터가 몇 개 안 될 때) - 예: 성별 등
- 비트를 직접 관리하므로 저장공간이 크게 감소하고 비트 연산 수행 가능
- Leaf 노드, ROWID 값들 대신 각 키 값에 대한 비트맵 저장
- OR 연산자를 포함하는 여러 개의 WHERE 조건을 자주 사용할 때 유리
- INSERT, UPDATE, DELETE와 같은 쿼리에서는 의미 X

#### 구성
> 인덱스 키값 + 시작 ROWID + 끝 ROWID + 비트맵 엔트리


<br/>

#### 🔗 참고
* [데이터베이스 인덱스 기초 개념 정리(인덱스의 정의, 특징, 사용 지침 등)](https://wkdtjsgur100.github.io/database-index/)
* [인덱스의 원리 및 종류](https://ssunws.tistory.com/45)