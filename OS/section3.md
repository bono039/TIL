# 명령
> 소스 코드와 명령어
> 
> 명령어의 구조와 주소 지정 방식

<br/>

## 소스 코드와 명령어
### 소스 코드와 명령어
<b>소스 코드</b>
- 고급 언어
- 개발자가 이해하고 실행하는 언어 ( =원시 코드)
<br/>

<b>명령어</b>
- 저급 언어
- 컴퓨터가 이해하고 실행하는 언어
- 종류
  1.  기계어 : 0과 1
  2. 어셈블리어 : 기계어를 읽기 편한 형태로 번역한 저급 언어
<br/>

### 컴파일 언어 vs 인터프리트 언어
<b>컴파일 언어</b>
* **소스 코드 > 컴파일러 > 목적 코드** <br/>
(고급 언어        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;               저급 언어)
- **`컴파일러`** : 컴파일 언어로 작성된 소스 코드를 저급 언어로 변환
- 한꺼번에 실행
 
<br/>

<b>인터프리터 언어</b>
* 인터프리터에 의해 한 줄씩 실행 
 
<br/>

## 명령어의 구조와 주소 지정 방식
### 명령어 구조
1. **`연산 코드`** : 수행할 연산

    - 종류
    1. **데이터 전송** <br/>
    -  MOVE, STORE, LOAD(FETCH), PUSH, POP
   
    2. **산술/논리 연산**
        
        - ADD / SUBTRACT / MULTIPLY / DIVIDE
        - INCREMENT / DECREMENT
        - AND / OR / NOT
        - COMPARE
        
    3. **제어 흐름 변경**
   
        -  JUMP                           : 특정 주소로 실행 순서 옮기기<br/>
        -  CONDITIONAL JUMP : 조건에 부합할 때 특정 주소로 실행 순서 옮기기<br/>
        -  HALT                            : 프로그램 실행 STOP<br/>
        -  CALL                            : 되돌아올 주소 저장한 채 특정 주소로 실행 순서를 옮기기<br/>
        -  RETURN                      : CALL 호출 시 저장했던 주소로 돌아가기
       
    5. **입출력 제어**
   
    -   READ (INPUT)       : 특정 입출력 장치로부터 데이터 읽기<br/>
    -   WRITE (OUTPUT) : 특정 입출력 장치로 데이터 쓰기<br/>
    -   START IO               : 입출력 장치 시작<br/>
    -   TEST IO                 : 입출력 장치 상태 확인

<br/>

2. <code>오퍼랜드</code>  : 연산에 사용될 데이터 / 연산에 사용될 데이터가 저장된 <b>위치 (= 주소 필드)</b>
    - 저장된 위치 사용 이유 : 명령어 내 표현 가능한 데이터 크기 제한 때문
    - 오퍼랜드 갯수 : 없는 경우도 있고, 하나 이상인 경우도 있음 (0-주소 ~ 3-주소 명령어)

<br/>

### 명령어 주소 지정 방식 (addressing modes)
> 유효 주소 = 연산에 사용될 데이터가 저장된 위치를 찾는 방법

① 즉시 주소 지정 방식 (immediate addressing mode)
![image](https://github.com/bono039/TIL/assets/67899934/c19e2a1d-bfd7-4772-81da-e62e4e525b69)

  - "연산에 사용할 데이터"를 오퍼랜드 필드에 직접 명시
  - 가장 간단한 형태
  - 연산에 사용할 데이터 크기는 작아질 수 있으나, 빠름

<br/>

② 직접 주소 지정 방식 (direct addressing mode)
![image](https://github.com/bono039/TIL/assets/67899934/03bab9dd-8597-4f10-89a1-c593b89e2cea)

  - 오퍼랜드 필드에 **"유효 주소"** 직접적으로 명시
  - 유효 주소를 표현할 수 있는 크기, 연산 코드만큼 줄어듦

<br/>

③ 간접 주소 지정 방식 (indirect addressing mode)
![image](https://github.com/bono039/TIL/assets/67899934/2e4aba7c-9b13-45fb-82d4-5400e04f6a67)

  - 오퍼랜드 필드에 **"유효 주소의 주소"** 명시
  - 속도 느림 (+ 메모리 접근을 최소화하는 것이 무조건 속도 면에서 좋다)

<br/>

④ 레지스터 주소 지정 방식 (register addressing mode)
![image](https://github.com/bono039/TIL/assets/67899934/2ae3c256-c050-4132-bb0d-cddee03dce01)

  - "연산에 사용할 데이터가 저장된 레지스터" 명시
  - <b>레지스터에 접근하는 속도 > 메모리에 접근하는 속도</b> <br/>
      → 일반적으로 **CPU 외부에 있는 메모리에 접근하는 것보다 CPU 내부에 있는 레지스터에 접근하는게 더 빠르다**

<br/>

⑤ **레지스터 간접 주소 지정 방식** (register indirect addressing mode)
![image](https://github.com/bono039/TIL/assets/67899934/ab999d67-bb5a-4ea2-bfd0-f4c527d2f8f0)

- "연산에 사용할 데이터를 메모리에 저장하고, 그 주소를 저장한 레지스터"를 오퍼랜드 필드에 명시

<br/>

#### 🔗 참고
* [[컴퓨터 공학 기초 강의] 6강. 소스코드와 명령어](https://www.youtube.com/watch?feature=shared&v=B8TDaBp3UWo)
* [[컴퓨터 공학 기초 강의] 7강. 명령어의 구조와 주소 지정 방식](https://youtu.be/bWPHUi6BPxo?feature=shared)
