---
layout: post
title: CS50_3 
desc: 
categories: development
tags: computerscience
comments: false
keywords: 컴퓨터사이언스기초, compuser science basic, CS50

---

# 컴퓨터 사이언스 기초

이 수업을 듣는 이유는 컴퓨터 사이언스가 무엇인지, 뭘 배우는지 기초를 쌓고 싶어서다. 
나는 컴퓨터의 컴자도 모르는 문과생으로 맨땅에 헤딩으로 데이터분석, 시각화, 머신러닝등을 배우고 실습, 프로젝트를 하였지만 항상 무언가 비어있다는 느낌?, 기초가 없다는 생각을 많이 했다.   

그러던 참 운이좋게 네이버 커넥트 재단에서 실시한 부스트 코딩뉴비 챌린지 프로그램을 우연히 알게되어 신청후 리더로 선발되었다. (하버드의 CS50강의와 함께 학습자료를 제공해 주는 좋은 코스이다!)  
일주일에 약2~3시간씩 총 6주 과정이다. 과정 자체가 짧고 얕아 다른 공부를 하기에 부담도 없어서 좋다.  

[Edwith 모두를 위한 컴퓨터 과학(CS50)](#https://www.edwith.org/boostcourse-cs-050/joinLectures/41307)  
[CS50](#https://cs50.harvard.edu/x/2020/)

# 알고리즘

목차  
[알고리즘 표기법](#알고리즘-표기법)  
[선형 검색](#선형-검색)  
[버블 정렬](#버블-정렬)  
[선택 정렬](#선택-정렬)  
[재귀](#재귀)  
[병합 정렬](#병합-정렬)  


# 알고리즘 표기법
수학 notation을 쓰지만 손짓과 같이 대강 표현하는 방법  
Big O  
Big Ω    

`Big O` (대문자, on the order of의 약자): ~만큼의 정도로 커지는것, 알고리즘의 실행시간을 나타내기 위해 사용(얼마나 오래 걸릴지!) 
- O(n^2)
- O(n log n)
- O(n) - 선형 검색
- O(log n) - 이진 검색
- O(1)  

`Big Ω` (omega): 알고리즘 실행 시간의 하한을 나타냄(얼마나 빨리 될 수 있을지!)  
- Ω(n^2)
- Ω(n log n)
- Ω(n) - 배열 안에 존재하는 값의 개수 세기
- Ω(log n)
- Ω(1) - 선형 검색, 이진 검색

예를 들어 선형검색에서 n개의 검색항목이 있을때 최대 n 번을 검색해야 하므로 상한은 O(n)이고 운 좋으면 한번에 찾을 수 있으니 하한은 Ω(1)

# 선형 검색

원하는 원소가 발견될 떄까지 처음부터 마지막 자료까지 차례대로 검색하는 알고리즘  
정확하지만 비효율적이다  
자료가 정렬되어 있지 않거나 그 어떤 정보도 없어 하나씩 찾아야 하는 경우에 유용  
탐색 이전에 정렬이 중요!

예시로 배열의 크기만큼 for 루프를 돌면서 배열의 인덱스를 차례대로 체크하는 코드다
```c
#include <stdio.h>

int main(void)
{
    // numbers 배열 정의 및 값 입력
    int numbers[] = {4, 8, 15, 16, 23, 42, 50};

    // 값 50 검색
    for (int i = 0; i < 7; i++)
    {
        if (numbers[i] == 50)
        {
            printf("Found\n");
            return 0;
        }
    }
    printf("Not found\n");
    return 1;
}
```

# 버블 정렬

어떤 배열이 주어졌을 때, 그 배열이 미리 정렬되어 있다면 훨씬 빠른 속도로 검색이 가능하다.  

버블 정렬은 두 개의 인접한 자료 값을 비교하면서 위치를 교환하는 방식으로 정렬하는 방법이다.
간단하지만 단 하나의 요소를 정렬하기 위해 너무 많이 교환하는 낭비가 발생할 수도 있다.

예를들어 6 4 7 1 3 5 2 를 오름차순으로 정렬하고 싶다하면  
 - 제일 먼저 6 과 4 비교  
 _4_ _6_ 7 1 3 5 2    
 - 6 과 7 비교  
 4 _6_ _7_ 1 3 5 2  
 - 7 과 1 비교  
 4 6 _1_ _7_ 3 5 2  
 - 7 과 3 비교  
 4 6 1 _3_ _7_ 5 2  
 - 7 과 5 비교  
 4 6 1 3 _5_ _7_ 2  
 - 7 과 2 비교   
 4 6 1 3 5 _2_ _7_    
 (7정렬 완료, 4~2는 다음 정렬 수행)

중첩 루프를 돌아야 하고 n개의 값이 주어지면 루프는 n-1, n-2번 반복하게 되므로 $(n−1)∗(n−2)=n^2−3n+2$ 번의 비교 및 교환이 필요    
그러므로 상한은 O(n^2), 하한도 정렬 여부랑 관계없기에 Ω(n^2)

다음은 0~9사이의 숫자 5개를 버블 정렬을 이용하여 오름차순으로 정렬하는 코드다
```c
#include <stdio.h>

int main(void) {  
  int number[5] = {1, 5, 3, 4, 2};
  int temp;
  for (int i=0; i < 5; i++)
  {
    for (int j=0; j < 5 - i - 1; j++ )
    {
      if (number[j] > number[j+1])
      {
        temp = number[j];
        number[j] = number[j+1];
        number[j+1] = temp;
      }
    }
  }

  for (int i=0; i < 5; i++)
  {
    printf("%d\n", number[i]);
  }
  return 0;
}
```


```python
# 파이썬으로 한번 해봤는데 훨씬 간단한거 같다
ls = [1, 5, 3, 4, 2]
for i in range(len(ls)-1):
    # length 가 5면 바깥루프는 4번
    for j in range(5-i-1):
        # 안쪽루프는 1번당 4 -> 3-> 2 -> 1
        if ls[j] > ls[j+1]:
            # 앞 값이 크면 교환
            ls[j], ls[j+1] = ls[j+1], ls[j]
print(ls)
```

    [1, 2, 3, 4, 5]


# 선택 정렬

선택정렬은 배열 안의 자료 중 가장 작은 수(or 가장 큰 수)를 찾아 첫번째 위치(or 마지막 위치)의 수와 교환해주는 방식의 정렬  
교환횟수는 최소화하나 각 자료를 비교하는 횟수는 증가  

예를들어 5 4 3 2 1 을 오름차순으로 정렬하고 싶다하면  

- 5에서 시작 4, 3, 2, 1 을 비교, 가장 작은 값 찾은후 리스트의 첫번째 값인 5와 교환  
_1_ 4 3 2 _5_  
- 1제외후 가장 작은 값 찾은후 두번째 리스트값인 4와 교환  
1 _2_ 3 _4_ 5

여기서도 두번의 루프를 돌아야 함   
바깥 루프는 순자들을 처음부터 방문하고, 안쪽 루프에서는 가장 작은 값을 찾아야함  
따라서 버블 정렬과 동일하게 소요 시간의 상한은 O(n^2), 하한도 마찬가지로 Ω(n^2)   
다만, n-1번의 교환만 필요하므로 버블 정렬의 교환 횟수보다는 적다  

다음은 숫자 5개를 선택정렬 하는 코드다  
- 배열 에서 가장 작은 값을 갖는 데이터를 찾고, 그 데이터를 배열의 맨 처음 데이터인 A[0]과 교환. 두번째 작은값은 A[1]과 교환...

```c
#include <stdio.h>
 
int main(void) {
  int num[5]={3, 5, 1, 4, 2};
  int i, j, temp=0;
 
  for(i=0; i<4; i++) {  
    for(j=0; j<4; j++) {  
      if(num[ j ]>num[ j+1 ]) {  
        temp=num[ j ];  
        num[ j ]=num[ j+1 ];  
        num[ j+1 ]=temp; 
      }  
    }  
  }
 
  for(i=0; i<5; i++) {  
    printf("%d",num[i]); 
  }
  printf("\n");
}

```


```python
# 파이썬으로 해봤다
ls2 = [3, 5, 1, 4, 2]

for i in range(len(ls2)-1):
    min_idx = i
    for j in range(i+1, len(ls2)):
        if ls2[min_idx] > ls2[j]:
            min_idx = j
            if min_idx != i:
                ls2[i], ls2[min_idx] = ls2[min_idx], ls2[i]

print(li)
```

    [1, 2, 3, 4, 5]


# 재귀

recursion: 함수를 함수내에서 재사용하는 방법  
함수가 본인 스스로를 호출해서 사용하는 개념이다  


```python
def factorial(n):
    if n == 1:      # n이 1일 때
        return 1    # 1을 반환하고 재귀호출을 끝냄
    return n * factorial(n - 1)    # n과 factorial 함수에 n - 1을 넣어서 반환된 값을 곱함
 
print(factorial(5))
```

    120


# 병합정렬


재귀를 활용해 정렬알고리즘을 더 효율적으로!

예시로 숫자들을 오름차순으로 정렬하자면

7 4 5 2 6 3 8 1  

- 먼저 숫자들을 반으로 나눔  

7 4 5 2 | 6 3 8 1  

- 그리고 나눠진 부분 중 첫번째를 한번 더 반으로 나눔  

7 4 | 5 2 | 6 3 8 1  

- 마지막으로 한 번 더 나눔  

7 | 4 | 5 2 | 6 3 8 1  


- 이제 숫자가 두 개 밖에 남지 않았으므로 더 이상 나누지 않고, 두 숫자를 다시 병합. 단, 이 때 작은 숫자가 먼저 오도록 함.   

4 7 | 5 2 | 6 3 8 1  


- 마찬가지로 5 2 부분도 반으로 나눈 후에 작은 숫자가 먼저 오도록 다시 병합

 4 7 | 2 5 | 6 3 8 1


- 이제 4 7과 2 5 두 개의 부분들을 병합 각 부분들의 숫자들을 앞에서 부터 순서대로 읽어들여 비교하여 더 작은 숫자를 병합되는 부분에 가져온다.

- 즉, 4와 2를 먼저 비교해서 2를 앞으로. 그 후에 4와 5를 비교해서 4를 앞으로.

- 그리고 7과 5를 비교해서 5를 앞으로, 남은 7을 뒤로.


2 4 5 7 | 6 3 8 1  


- 오른쪽 4개의 숫자들도 위와 동일한 과정 

2 4 5 7 | 1 3 6 8

- 마지막으로 각각 정렬된 왼쪽 4개와 오른쪽 4개의 두 부분을 병합

- 2와 1을 비교하교 1을 앞으로, 2와 3을 비교하고 2를 앞으로, 4와 3을 비교하고, 3을 앞으로....이 과정으로 정렬완료
 

1 2 3 4 5 6 7 8


전체 과정을 요약해서 도식화해보면 아래와 같다.

 

7 | 4 | 5 | 2 | 6 | 3 | 8 | 1 → 가장 작은 부분 (숫자 1개)으로 나눠진 결과

4   7 | 2   5 | 3   6 | 1   8 → 숫자 1개씩을 정렬하여 병합한 결과

2   4   5   7 | 1   3   6   8 → 숫자 2개씩을 정렬하여 병합한 결과

1   2   3   4   5   6   7   8 → 마지막으로 숫자 4개씩을 정렬하여 병합한 결과

병합 정렬 실행 시간의 상한은 O(n log n)이다.  

숫자들을 반으로 나누는 데는 O(log n)의 시간이 들고, 각 반으로 나눈 부분들을 다시 정렬해서 병합하는 데 각각 O(n)의 시간이 걸리기 때문이다.  

실행 시간의 하한도 역시 Ω(n log n) 이다. 숫자들이 이미 정렬되었는지 여부에 관계 없이 나누고 병합하는 과정이 필요하기 때문이다.



예시 코드
```c
#include <stdio.h>

int temp_arr[9];

void merge(int *arr, int start, int middle, int end){
	int i = start;
	int j = middle + 1;
	int k = start;
	
	//비교하여 데이터정렬 및 삽입
	while(i<=middle && j<=end){
		if(arr[i]<=arr[j]) temp_arr[k] = arr[i++];
		else temp_arr[k] = arr[j++];
		k++;
	}
	
	//남은 데이터 삽입
	if(i>middle){
		for(int t=j;t<=end;++t){
			temp_arr[k] = arr[t];
			++k;
		}
	}
	else{
		for(int t=i;t<=middle;++t){
			temp_arr[k] = arr[t];
			++k;
		}
	}
	
	//임시 저장용 배열에 저장된 값을 원래 배열에 넣어줌
	for(int t=start;t<=end;++t){
		arr[t] = temp_arr[t];
	}
}


void mergeSort(int *arr, int start, int end){
	//크기가 1 일대 까지 호출, 1단위 까지 쪼갬
	if(start < end){
		int middle = (start + end) / 2;
		mergeSort(arr, start, middle);
		mergeSort(arr, middle+1, end);
		//다시 병합
		merge(arr, start, middle, end);
	}
}
int main(){
	
	int i;
	//무작위 배열
	int arr[8] = {5, 6, 2, 4, 3, 8, 7, 1};
	
	//배열, 배열시작위치 0, 배열 마지막위치 7 
	mergeSort(arr, 0, 7);
	
	//출력 
	for(i=0;i<8;++i){
		printf("%d\n", arr[i]);
	}
	
	return 0;
}

```

# 문제
숫자 애너그램 만들기  
애너그램이란 문자를 재배열해 다른 뜻을 가진 단어로 바꾸는것을 말함.
5자리 숫자 1쌍을 입력으로 주었을때 애너그램 일경우 true, 아닐경우 false를 출력해보자.  
예)  
입력값: 12345, 54321 -> 출력값: True  

```c
#include <stdio.h>
 

int* sort(int number[]);//버블 정렬

int main(void)
{
    int number1[5] = {1,2,3,4,5};
    int number2[5] = {2,5,4,3,1};  
    int* sortnumber1 = sort(number1);//정렬된 변수를 저장
    int* sortnumber2 = sort(number2);

    for (int i = 0; i <5; i++)
    {
        if (sortnumber1[i] != sortnumber2[i]) //정렬된 배열에서 하나의 요소라도 다를경우 false출력
        {
            printf("False\n");
            return (0);
        }
    }
    printf("True\n");

   return 0;


}

int* sort(int number[])
{
    int temp; //임시로 값 저장
    
    for (int i = 0; i <5;i++)
    {
        for (int j = 0; j < 5-i-1;j++)
        {
            if (number[j]>number[j+1])
            {
                temp = number[j];
                number[j] = number[j+1];
                number[j+1] = temp;
            }
        }
    }
    return number;
}

```
