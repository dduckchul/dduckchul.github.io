---
layout: post
title: C#의 함수형 프로그래밍 Lambda와 Event
subtitle : 근데 이제 자바를 곁들인
author: dduckchul
tags : [C#, Lambda, 함수형프로그래밍, 메타버스부트캠프]
published: false
---
# C#의 람다식
* 람다 식을 사용하여 익명함수를 만든다
* 이전에는 delegate 연산자를 사용하여 익명 메서드(?)를 만들었던거 같으나, 람다식을 지원한 이후로는 굳이 저렇게 안해도됨
```cs
// old C# codes
Func<int, int, int> sum = delegate (int a, int b) { return a + b; };
Console.WriteLine(sum(3, 4));  // output: 7

// using Lambda
Func<int, int, int> sum = (a, b) => a + b;
Console.WriteLine(sum(3, 4));  // output: 7
```
* 익명함수 : 이름이 없는 함수 보통 짧은 코드의 델리게이트 대입 등에 사용한다.
* (입력 매개 변수) => {실행 코드}

## 모든 람다식은 대리자 형식으로 변환 할 수 있다.
* 람다식이 값을 반환하지 않으면 Action, 값을 반환한다면 Func로 변환 가능하다.

```cs
Action<int> printString = a =>
{
    Console.WriteLine("아아 제가 한번 출력을 해보겄습니다잉");
    Console.WriteLine(a.ToString());
};

Func<int, int, string> calcAndToString = (a, b) => (a + b).ToString();
Func<int, int, string> calcAndToString2 = (a, b) => (a + b).ToString();

printString(10);
Console.WriteLine("출력을 해봅시다 : " + calcAndToString(4, 6));
```

// 캡쳐, 클로저?
https://lunatk.github.io/2020/07/27/20200727-closure-and-free-vars/

