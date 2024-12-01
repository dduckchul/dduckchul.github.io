---
layout: post
title: 함수 심화 문제 풀어보기, 재귀 함수의 위험성
subtitle: 심화문제 풀면서 다시 되짚어보는 재귀함수의 위험성..
author: dduckchul
tags : [C#, 함수, 메타버스부트캠프]
---
# 심화 1 - 복합조건을 가진 함수 제작
  * 인자값으로 정수형 하나가 주어지면, 숫자 1에서부터 인자값으로 전달받은 숫자 사이의 모든 
자연수 중, 3의 배수이거나 5의 배수인 수들의 합을 구하여  
정수형으로 반환하는 함수를 작성하세요
  * 함수 중에 3과 5를 구분해서 인트를 반환하는 함수를 추가해준다.

```cs
// 심화 1. 복합 조건을 가진 함수 제작
public int Sum3or5MultiplesToTarget(int target)
{
    int sum = 0;
    if (target < 0)
    {
        Console.WriteLine("음수는 안됩니다");
        return sum;
    }

    for (int i = 0; i <= target; i++)
    {
        sum += Filter3or5Multiples(i);
    }
    return sum;
}
// 3과 5 구분해주는 함수 추가 구현
public int Filter3or5Multiples(int i)
{
    if (i % 3 == 0 || i % 5 == 0)
    {
        return i;
    }
    return 0;
}
```
------------------
# 심화 2 - 자릿수의 합 인코더 제작
* 인자값으로 1이상의 수를 입력받았을 때, 주어진 정수의 각 자리 수를 더한 결과를 정수형으로 반환하는 함수를 작성하세요. 예를 들어, 5611은 5+6+1+1 로 13을 반환하는 함수입니다.

## 의사코드를 짜보자
1. 인자값으로 1 이상의 수를 입력 받는다 -> int 형으로 함수를 전달.
2. 주어진 정수의 각 자리수를 분리한다.
  * 뇌 없는 코딩 - > case 별로 10억자리 까지 나누기로 노가다 반복! 🤣
  * 쉬운 꼼수쓰기 -> 스트링으로 변환, 스트링의 문자 배열을 각각 인트로 변환해서 더하기
  * 자릿수 별로 짜르기..?
3. 각각 나온 자릿수를 더해서 반환해 준다.  

```cs
// 심화 2. 자릿수 합의 디코더
// 2-2번 방법!
public int DigitsSumEncoderByConvertStr(int target)
{
    int result = 0;
    string intStr = target.ToString();
    char[] digitCharArray = intStr.ToCharArray();
    
    foreach (char digit in digitCharArray)
    {
        result += int.Parse(char.ToString(digit));
    }
    return result;
}
```
## 근데 이거 너무 꼼수아님!? 😅
* 선생님께 힌트 요청.. 
* 자릿수 별로 짜르기..? -> 10의 제곱이 반복되는 패턴..
* 오! 제곱으로 표현해서 나눈다면.... int 의 맥스값은 21억이니까 10번 반복
* (7543 / 10^3) % 10 = 7 % 10 = 7;
* (7543 / 10^2) % 10 = 75 % 10 = 5;
* (7543 / 10^1) % 10 = 754 % 10 = 4;
* (7543 / 10^0) % 10 = 7543 % 10 = 3; 

### 개선 버전 
```cs
// 근데 C#은 제곱연산 ^을 지원하지 않는다.. 힝.. -> 비트 연산이 되어버림
// math로 일단 해결, 추후에 봅시다
public int DigitsSumEncoder(int target)
{
    int result = 0;
    for (int i = 0; i < 10; i++)
    {
        result += target / (int) Math.Pow(10, i) % 10;
    }
    return result;
}
```
-----------------
# 심화 3 - 피보나치 수열 만들기
* 피보나치 수열은..? [1,1,2,3,5,8....] 
맨 처음 두개의 1을 제외하면, 앞자리 두개의 숫자의 합이 그 다음 자리의 숫자가 되는 수열

## 재귀함수 버전
```cs
// f0 = 0
// f1 = 1
// f2 = 0(f0)+1(f1)
// f3 = 1(f1)+1(f2)
// f4 = 1(f2)+2(f3)
// f5 = 2(f3)+3(f4)
public int FibonacciFunction(int target)
{
    // 0보다 작을 경우 0 반환
    if (target <= 0)
    {
        return 0;
    }
    // 1일 경우는 1을 반환
    if (target == 1)
    {
        return 1;
    } else
    {
        return FibonacciFunction(target-2) + FibonacciFunction(target-1);                
    }
}
```  

* 그런데 재귀 함수로 짰을때.. target 값이 50만 넘어가도 내 로컬에서는 돌지가 않는다... 헉...
* 이유는..? 재귀 함수로 두번의 함수를 호출 하기 때문에, 계산 해야될 함수의 숫자는 2^n 개가 되버리는 구조
  * O(2^n)으로 표현한다.
* 변수를 저장할 공간이 아예 없다면 쓸수도 있겠지만 너무너무 위험함..

## 피보나치 수열 For-loop 버전
```cs
public int FibonacciFunctionLoop(int target)
{
    int result = 0;
    int f0 = 0;
    int f1 = 1;
    if (target <= 2)
    {
        return f1;
    }
    for (int i = 2; i <= target; i++)
    {
        result = f0 + f1;
        f0 = f1;
        f1 = result;
    }
    return result;
}
```
* result 값과, 앞의 두개를 기억 할 수 있는 변수 두가지가 있다면.. 
* 동일한 결과를 얻지만 시간 복잡도가 O(n)인 코드로 개선할 수 있다.

### 추가적으로 재귀 함수의 위험성...
```cs
        return FibonacciFunction(target-1) + FibonacciFunction(target);
```  
* 재귀 함수 버전에서 초기에 생각없이 짠 코드는 위와 같은 코드
* 이렇게 되었을경우 바로 스택 오버 플로우로 터져버린다.
* 이유는..? 
  * target=3 일때 피보나치 함수가 줄어들 수가 없기 때문에, 무한으로 함수 스택을 쌓게 되는듯..?

### 이렇듯.. 재귀 함수의 위험성을 다시 짚어 보자면
1. 중단점이 없는 재귀함수는 큰 장애를 일으킬 수 있다.
2. 위의 함수처럼 함수 두개를 부르는 형태로 짰다면, 알고리즘의 시간 복잡도가 기하급수적으로 늘어나게 된다.
3. 재귀 함수는 보통 가독성이 그렇게 좋지 않기때문에, 코드 오류시 읽기 어려워진다.