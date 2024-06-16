# 기수 정렬
> 1. 정의
> 2. 시간 복잡도
> 3. 장단점
> 4. 참고

<br/>

## 정의
> 데이터를 구성하는 <b>기본 요소(Radix)</b>를 이용하여 낮은 자리(1의 자리)부터 비교해 정렬하는 방식
- 중간 결과 저장할 <b>bucket</b>이 필요함!
- 계수 정렬(Counting Sort)의 비효율성을 개선하고자 Radix Sort를 사용할 수 있음.
- 자릿수의 값 별로 정렬하므로, 나올 수 있는 값의 최대 사이즈는 9 (범위 : 0~9)


#### 예제 코드
```java
public void Radix_Sort(int[] data) {
  int maxsize = getMaxlength(data);
  ArrayList<Linear_Queue> bucket = new ArrayList();
  
  int powed = 1;
  int index = 0;
  
  // 버킷 생성
  for(int i = 0 ; i < 10 ; i++) {
      bucket.add(new Linear_Queue(10));
  }
  
  for(int i = 1 ; i <= maxsize ; i++) { // 자리수가 가장 큰 수만큼 전체 반복문 반복
      for(int j = 0 ; j < data.length ; j++) {
          bucket.get((data[j]/powed)%10).enqueue(data[j]);  // 각 자리수의 맞는 index의 bucket에 넣는다.
      }
  
      // 버킷에서 데이터 가져오기
      for(int k = 0 ; k < 10 ; k++) {
          int bucket_num = bucket.get(k).rear;
  
          for(int n = 0 ; n <= bucket_num ; n++) {
              data[index] = bucket.get(k).dequeue();
              index++;
          }
      }
      index =0;
      powed *= 10;
  }
}

public int getMaxlength(int[] data) {
    int maxsize = 0;

    for(int i = 0 ; i < data.length ; i++) {
        int length = (int)Math.log10((double)data[i])+1;
        if(maxsize < length) {
            maxsize = length;
        }
    }
    return maxsize; // 가장 큰 자리수 반환
  }
}

```

- Queue 자료구조 Class
```
public class Linear_Queue {
    
    int rear = -1;
    int front = 0;
    int maxsize = 0;
    int[] Linear_Queue;
    
    public Linear_Queue(int maxsize) {
        this.maxsize = maxsize;
        Linear_Queue = new int[maxsize];
    }
    
    public void enqueue(int num) {
        if(rear != maxsize-1) {
            Linear_Queue[++rear] = num;
        }
        else {
            System.out.println("데이터 다참");
        }
    }
    
    public int dequeue() {
        if(rear!=front || (rear==0 && front==0)) {
            int tmp = Linear_Queue[front];
            for(int i=1;i<=rear;i++) {
                Linear_Queue[i-1] = Linear_Queue[i];
            }
            rear--;
            return tmp;
        }
        else {
            return -1;
        }    
    }
}
```
<br/>

## 시간 복잡도
- <b>O(dn))</b>
⇒ <b>d</b> : 정렬할 숫자의 자릿수 / <b>n</b> : 정렬될 수의 갯수

<br/>

## 장단점
* 장점
   * 문자열, 정수 정렬 가능
   * 정렬 속도 빠름

* 단점
  * 자릿수가 없는 것은 정렬 불가 (부동 소숫점)
  * 추가적 메모리가 많이 필요하다.

<br/>

## 참고
- [기수 정렬(Radix sort)](https://gyoogle.dev/blog/algorithm/Radix%20Sort.html)
- [[정렬] 7. 한 살도 이해하는 기수 정렬(radix sort)](https://10000cow.tistory.com/entry/%EC%A0%95%EB%A0%AC-7-%EA%B8%B0%EC%88%98-%EC%A0%95%EB%A0%ACradix-sort)
- [06 정렬 알고리즘 - 기수 정렬(Radix Sort)](https://lktprogrammer.tistory.com/48)
