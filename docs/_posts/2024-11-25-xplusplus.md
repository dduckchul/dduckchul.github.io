---
layout: post
title: 후위 증가 연산자, 꼭 잘 알아보고 쓰자!
author: dduckchul
tags : [컴퓨터공학, 메타버스부트캠프]
---

C# 프로그래밍 학습중.. 
아래와 같은 코드를 따라 치다가 문득 이상한 점이 보인다.

```cs
    int x = 1;

    // 선생님이 치셨던 코드 x++;
    // 의도한것 x += 1;
    // 실제로 친 코드
    x = x++;
    
    Console.WriteLine("========================= 왜 1이 나올까요..?");
    Console.WriteLine(x);
```

### 위 값을 실제 컴파일 후 실행해보면 1로 나온다.

## 뭐지?! 난 바본가?
* 선생님의 설명은 아주 좋은 질문, 언어별로 다를 수 있다.
* 헉.. 자바에서 그럼 내가 저렇게 썼던가?

```java
int plusable = 0;
plusable = plusable++;
// 자바도 0이네?!
System.out.println("plusable은? : " + plusable);
```
### 자바도 0 이었다.. 😂😂😂😂😂
* 어떻게 된것인지 좀 더 자세히 알아보자..

## 마이크로 소프트 공식 문서
- [연산자우선순위](https://learn.microsoft.com/ko-kr/dotnet/csharp/language-reference/operators/#operator-precedence)
- [후위증가연산자](https://learn.microsoft.com/ko-kr/dotnet/csharp/language-reference/operators/arithmetic-operators#increment-operator-)
* 아니 마소 선생님들 연산자 우선순위로는 분명히 ++ 연산이 먼저인데요..?

## 그러나 그것이 문제가 아닌거같다
* [java버전질문](https://stackoverflow.com/questions/7911776/what-is-x-after-x-x)
* [c#버전질문](https://stackoverflow.com/questions/226002/whats-the-difference-between-x-x-vs-x)

### 추가는 내일 아침 선생님이 설명해주시는 내용도 들어보자!