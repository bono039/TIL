# CPU의 성능 향상 기법
> 빠른 CPU를 위한 설계 기법
> 
> 명령어 병렬 처리 기법
>
> 명령어 집합 구조, CISC와 RISC

<br/>

## 빠른 CPU를 위한 설계 기법
### 1. 클럭
```
컴퓨터의 모든 부품을 움직이게 하는 시간 단위
```

<img src="https://github.com/user-attachments/assets/60d3ff42-36ef-4b2c-a65a-1716fca463cf" alt="clock" width="500"/>

- 속도 : **Hz** 단위로 측정
  - ```Hz``` : 1초에 클럭이 반복되는 횟수
- 클럭 속도 높이는 방법 外 코어/스레드 수 늘리는 방법 있음

<br/>

### 2. 코어와 멀티 코어
```
CPU 내에서 명령어를 실행하는 부품 (여러 개 가능)
```
- **멀티코어 프로세서** : 여러 개의 코어를 포함하고 있는 CPU
![image](https://github.com/user-attachments/assets/0a02e2cd-f055-461a-8a20-0ba23226e40a)

<br/>

### 3. 스레드와 멀티 스레드
```
실행 흐름의 단위
```

#### 1. HW 스레드
> 하나의 코어가 **동시에 처리**하는 명령어 단위 (= 논리 프로세서)
> 
> <img src="https://github.com/user-attachments/assets/88511986-8986-46b5-a22d-8ed69122421c" alt="hwThread" width="350"/>

- 멀티 스레드 프로세서, 멀티 스레드 CPU
  - 멀티 스레드 프로세서 설계 시 가장 큰 핵심 : **_레지스터_**

<br/>

#### 2. SW 스레드
> 하나의 프로그램에서 **독립적**으로 실행되는 단위
![image](https://github.com/user-attachments/assets/75af54d4-d03e-4367-a492-520e21ab0f44)

<br/>

## 명령어 병렬 처리 기법
### 명령어 파이프라이닝
```
동시에 여러 개의 명령어를 겹쳐 실행하는 기법
```

<img src="https://github.com/user-attachments/assets/627dc591-154f-4d53-8bbd-a37732aa4378" alt="instruction" width="400"/>

- 명령어 처리 과정
    1. 명령어 **인출** (Instruction Fetch)
    2. 명령어 **해석** (Instruction Decode)
    3. 명령어 **실행** (Execute Instruction)
    4. 결과 **저장** (Write Back)
- 같은 단계가 겹치지 않는다면, CPU는 '각 단계 동시 실행 가능'

<br/>

### 파이프라인 위험
```
명령어 파이프라인이 성능 향상에 실패하는 경우
```

#### 1. 데이터 위험
- 원인 : 명령어 간 의존성
- 모든 명령어 동시 처리 불가 (이전 명령어를 끝까지 실행해야만 비로소 실행 가능한 경우)
<img src="https://github.com/user-attachments/assets/ae05a70f-485e-4c29-a2d1-df69326500f4" alt="instruction" width="500"/>

<br/>

#### 2. 제어 위험
- 프로그램 카운터의 갑작스러운 변화
- 분기 예측

#### 3. 구조 위험
- 서로 다른 명령어가 같은 CPU 부품(ALU, 레지스터)을 쓰려고 할 때

<br/>

### 슈퍼스칼라
```
CPU 내부에 여러 개의 명렁어 파이프라인을 포함한 구조
```
- 파이프라인 위험도 증가로 인해, 파이프라인 개수에 비례해 처리 속도가 증가하진 않음

<br/>

### 비순차적 명령어 처리
```
파이프라인 중단 방지를 위해 명령어를 순차적으로 처리하지 않는 명령어 병렬 처리 기법
```
- **_= 합법적 새치기_**
- 의존성이 없는 명령어의 순서를 바꾸는 것만으로도 파이프라인 중단 방지 가능
- 전체 프로그램 실행 흐름에는 영향 X

<br/>

#### 🔗 참고
* [[컴퓨터 공학 기초 강의] 12강. 빠른 CPU를 위한 설계 기법](https://youtu.be/VO0RQAA7KYc?feature=shared)
* [[컴퓨터 공학 기초 강의] 13강. 명령어 병렬 처리 기법](https://youtu.be/Btsa_U-f26k?feature=shared)
* [[컴퓨터 공학 기초 강의] 14강. 명령어 집합 구조, CISC와 RISC](https://youtu.be/3Yz7OnVUM28?feature=shared))
