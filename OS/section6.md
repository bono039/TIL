# 메모리와 캐시 메모리
> RAM의 특징과 종류
> 
> 메모리의 주소 공간
>
> 캐시 메모리

<br/>

## RAM의 특징과 종류
### 메모리
- 주기억장치 : **휘발성 저장 장치** ▷ **RAM** (→ 메모리), ROM
- 보조기억장치 : 비휘발성 저장 장치

### RAM 특징
- 휘발성 저장 장치 (전원 꺼지면 내용 날아감)
- 많은 프로그램들 동시 실행에 유리

### RAM 종류
#### 1. DRAM (Dynamic RAM)
- 저장된 데이터가 **동적으로 사라지는 RAM**
- 데이터 소멸 막기 위해 주기적으로 refresh해야 함
- 일반적으로 메모리로 사용되는 RAM
     - 상대적으로 소비전력이 낮고, 저렴하고, 집적도가 높아 대용량으로 설계하기 용이하므로

#### 2. SRAM (Static RAM)
- 저장된 데이터가 **정적인 (사라지지 않는) RAM**
- DRAM보다 더 빠름
- 상대적으로 소비전력이 높고, 비싸고, 집적도가 낮아 '대용량 설계 필요 없이 빨라야 하는 장치'에 사용
<img src="https://github.com/user-attachments/assets/95c6aa01-fa64-42ae-b7ce-f5a09ba87a20" alt="clock" width="450"/>

<br/>

#### 3. SDRAM (Synchronous RAM)
- 특별한(발전된 형태의) DRAM
- 클럭 신호와 동기화된 DRAM

<br/>

#### 4. DDR SDRAM (Double Data Rate SDRAM)
- 특별한(발전된 형태의) SDRAM
- 최근 가장 대중적으로 사용하는 RAM
- **대역폭**을 2배 넓혀 속도를 빠르게 만든 SDRAM &nbsp; (* _대역폭_ : 데이터를 주고받는 길의 너비)
          <details>
               <summary>각각 대역폭</summary>
               <div markdown="1">
               
     → SDR SDRAM : 대역폭 1개          
     → DDR SDRAM : 대역폭 2개          
     → DDR2 SDRAM : 대역폭 4개          
     → DDR3 SDRAM : 대역폭 8개          
     → DDR4 SDRAM : 대역폭 16개
               

<br/>

## 메모리의 주소 공간
### 물리 주소
- 메모리 입장에서 바라본 주소
- 정보가 실제로 저장된 HW 상 주소

### 논리 주소
- CPU와 실행 중인 프로그램 입장에서 바라본 주소
- 실행 중인 프로그램 각각에게 부여된 0번지부터 시작하는 주소
<img src="https://github.com/user-attachments/assets/45c67e98-d094-420a-bc45-5fdceeb8c8f1" alt="clock" width="450"/>

<br/>

#### 🔗 참고
* [[컴퓨터 공학 기초 강의] 15강. RAM의 특성과 종류](https://youtu.be/VO0RQAA7KYc?feature=shared)
* [[컴퓨터 공학 기초 강의] 16강. 메모리의 주소 공간-물리 주소와 논리 주소](https://youtu.be/_mQNCRA1fVA?feature=shared)
* [[컴퓨터 공학 기초 강의] 17강. 캐시 메모리](https://youtu.be/qLCP0PwRp_w?feature=shared)
