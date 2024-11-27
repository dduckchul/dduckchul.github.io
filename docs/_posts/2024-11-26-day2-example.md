---
layout: post
title: Day2 제어문, 반복문 연습문제
author: dduckchul
tags : [C#, 반복문, 제어문, 메타버스부트캠프]
---

# 과제 4. 별찍기 기능 구현

- 중첩반복문을 활용하여 아래 그림처럼 출력하는 네 가지 프로그램을 각각 작성하여 보자.
- `Tip` : `Console.Write(" ");`를 쓰면 빈 공백 하나를, `Console.Write("*");`을 쓰면 별 하나를 출력할 수 있다

![alt text](/assets/img/posting/2024/starprint.webp)


```cs
/* 
    1. 반복문을 n회 반복한다
    1.1 5회, 혹은 그 이상의 숫자를 입력 받을 수 있도록 설정한다. 
    2. 1번과 3번은 별을 먼저 출력한다, 2번과 4번은 공백을 먼저 출력한다.
    3. 별 혹은 공백을 출력 한 갯수를 임시 변수에 저장한다.
    4. 다음 출력할 별 혹은 공백을 출력한다.
    5. 반복의 마지막에 한줄을 띄어준다.
*/
public void StarPrintProgram()
{
    Console.WriteLine("n x n 별 출력 프로그램 입니다, 정수를 입력하세요");
    int max = int.Parse(Console.ReadLine());            
    Console.WriteLine("==================== case1");
    // case 1.
    for (int temp = 0; temp < max; temp++)
    {
        int stared = 0;
        for (int star = 0; star <= temp; star++)
        {
            // stared = stared + 1;
            stared = star;
            Console.Write("*");
        }

        for (int blank = stared + 1; blank < max; blank++)
        {
            Console.Write(" ");
            //Console.Write("ㅁ"); // 디버깅용              
        }

        Console.WriteLine();
    }
    
    Console.WriteLine("==================== case3");
    // case 3.
    for (int temp = 0; temp < max; temp++)
    {
        int stared = 0;
        for (int star = max; star > temp; star--)
        {
            stared = stared + 1;
            Console.Write("*");
        }

        for (int blank = stared; blank < max; blank++)
        {
            Console.Write(" ");
            // Console.Write("ㅁ"); 디버깅용                                       
        }

        Console.WriteLine();
    }
    
    Console.WriteLine("==================== case2");
    // case 2.
    for (int temp = 0; temp < max; temp++)
    {
        int blanked = 0;
        int needToBeBlank = max - temp - 1;
        
        for (int blank = 0; blank < needToBeBlank; blank++)
        {
            blanked = blanked + 1;
            Console.Write(" ");                    
            // Console.Write("ㅁ"); 디버깅용                   
        }

        for (int star = blanked; star < max; star++)
        {
            Console.Write("*");
        }

        Console.WriteLine();
    }
    
    // case 4.
    Console.WriteLine("==================== case4");
    for (int temp = 0; temp < max; temp++)
    {
        int blanked = 0;
        for (int blank = 0; blank < temp; blank++)
        {
            blanked = blanked + 1;
            Console.Write(" ");                    
            // Console.Write("ㅁ"); 디버깅용                   
        }

        for (int star = blanked; star < max; star++)
        {
            Console.Write("*");
        }

        Console.WriteLine();
    }            
}
```

# 심화 과제 2. 입력을 통한 다이아몬드 출력 기능 구현

- 출력할 다이아몬드 형태를 사용자로부터 입력 받은 후,  
만약 짝수일경우 홀수를 다시 입력하라고 유저에게 무한 반복으로 요구한다.
- 홀수가 입력되었을 경우,  
다이아몬드 중간 부분이 유저의 입력과 같은 다이아몬드를 출력하는 프로그램 제작.

![출력 예제](/assets/img/posting/2024/diamond.webp)

``` cs
/*
    1. 다이아몬드를 그릴 줄 수를 입력한다
    1.1 홀수가 들어 올 때까지 입력을 무한루프로 반복하여 검증한다.
    2. 다이아몬드를 수식에 따라 그린다
    2.1 수식을 두 부분으로 나눈다
    2.2 늘어나는 부분까지 그린다
    2.3 줄어드는 부분을 그린다.
*/
public void OutputDiamondProgram()
{
    // 그려야할 다이아몬드
    int diamondNumber = 0;
    // 현재 다이아몬드 높이 저장
    int diamondHeight = 0;            
    bool isDrawable = false;

    Console.WriteLine("출력할 다이아몬드를 홀수로 입력하세요");
    isDrawable = int.TryParse(Console.ReadLine(), out diamondNumber);

    while (!isDrawable || diamondNumber <= 1 || diamondNumber % 2 == 0)
    {
        Console.WriteLine("1이 아닌 홀수로 입력해주세요");
        isDrawable = int.TryParse(Console.ReadLine(), out diamondNumber);                
    }
    
    // 0번째 -> 0 +1
    // 1번째 -> 0 +1 + 2*1
    // 2번째 -> 0 +1 + 2*2
    // 3번째 -> 0 +1 + 2*3
    // 4번째 -> 0 +1 + 2*2
    // 5번째 -> 0 +1 + 2*1
    // 6번째 -> 0 +1 
    
    //// 다이아몬드 그리기
    for (diamondHeight = 0; diamondHeight < diamondNumber; diamondHeight++)
    {
        // 그려야할 별 숫자가 1->3->5->7->5->3->1로 바껴야하는데.... 흠....
        int diamondStar = 0;

        if (diamondHeight * 2 < diamondNumber)
        {
            diamondStar = 1 + (diamondHeight * 2);                    
        }
        else // 이게 맞냐... ㅠㅠㅠㅠ 더 좋은 식이 생각이 안남
        {
            diamondStar = (diamondNumber - diamondHeight - 1) * 2 + 1;
        }

        // 그려야할 공백 : 3,3 -> 2,2 -> 1,1 -> 0,0 -> 1,1 -> 2,2 -> 3,3 으로 변경
        int blanks = (diamondNumber - diamondStar) / 2;
        
        for (int i = 0; i < blanks; i++)
        {
            Console.Write(" ");
        }
        for (int j = 0; j < diamondStar; j++)
        {
            Console.Write("*");
        }
        for (int i = 0; i < blanks; i++)
        {
            Console.Write(" ");
        }
        Console.WriteLine("");
    }
}
```