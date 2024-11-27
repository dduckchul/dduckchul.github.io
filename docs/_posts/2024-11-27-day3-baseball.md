---
layout: post
title: 숫자 야구 문제
author: dduckchul
tags : [C#, 반복문, 제어문, 메타버스부트캠프]
---
# 숫자 야구 구현
![숫자야구](/assets/img/posting/2024/baseball_example.jpeg)

## 숫자 야구를 의사 코드를 통해 구현해보자..
* 아직 배운 과제는 반복문, 제어문뿐....
* 배열없이, 반복문, 제어문과 사칙 연산, 논리 연산으로만 만들어보자  

### 숫자 야구 만들기
1. 임의의 세자리 숫자 만들기
    * int 3개로 변수 만들기 -> rand 1~10
    * int 3개를 만드는중, 앞의 변수와 중복되면 다시 만든다.
2. 유저가 입력한 세가지 숫자를 받는다.
    * 123 -> 각 자리수를 어떻게 세가지 인트로 받을까..?
        * 923 / 100 = 1 100의 자리, 나눗셈 몫 구하기
        * 923 / 10 = 92, 92 / 10 = 9 10의 자리 몫 구하기
        * 923 % 100 = 23, 23 % 10 = 3 1의자리의 나머지 구하기
    * 위 과정 중 앞의 변수와 중복되면 다시 입력해달라고 한다.
3. 생성한 변수 c1,c2,c3와 유저 변수 u1,u2,u3를 비교한다.
4. 스트라이크와 볼 변수를 추가한다.
5. 스트라이크와 볼을 비교한다
- c1과 u1, u2, u3을 비교한다.
- c2와 u1, u2, u3을 비교한다.
- c3와 u1, u2, u3을 비교한다.
  * 가정중에 중복된 수가 없다고 했으니,  
  한번씩만 비교해도 스트라잌과 볼 판단이 된다.
6. 위 과정을 11회 반복한다.
  - 스트라이크가 3개라면, break 유저 승 ㅊㅋㅊㅋ
  - 11회 끝나기 전까지 못맞췄다면 컴퓨터의 승

```cs
        public void BaseBallGame()
        {
            // 랜덤 객체 생성
            Random randomNumGenerator = new Random();
            
            // 컴퓨터가 정수 생성
            int computerNum1 = randomNumGenerator.Next(1, 10);
            int computerNum2 = 0;
            int computerNum3 = 0;
            
            bool isNumberGenerating = true;
            while (isNumberGenerating)
            {
                computerNum2 = randomNumGenerator.Next(0, 10);
                computerNum3 = randomNumGenerator.Next(0, 10);
                if (computerNum1 == computerNum2)
                {
                    continue;
                }
                if (computerNum2 == computerNum3)
                {
                    continue;
                }
                if (computerNum1 == computerNum3)
                {
                    continue;
                }

                isNumberGenerating = false;
                Console.WriteLine($"디버깅용 만든 값 : {computerNum1}{computerNum2}{computerNum3}");
            }
            
            // 11 이닝동안 게임 진행
            int game = 11;
            bool isUserWin = false;
            
            Console.WriteLine("👍신나는 야구 게임 👍");
            
            for (int i = 0; i < game; i++)
            {
                // 유저 숫자 입력 및 검증
                int userNum1 = 0;
                int userNum2 = 0;
                int userNum3 = 0;
                int userNumber = 0;
                bool isUserValidating = true;
                Console.WriteLine($"{i+1} 이닝.....");
                Console.WriteLine("세자리 숫자를 입력해주세요");
                Console.WriteLine("-------------------");
                while (isUserValidating)
                {
                    userNumber = int.Parse(Console.ReadLine());
                    // 이전 결과 클리어
                    Console.Clear();
                    userNum1 = userNumber / 100;
                    userNum2 = (userNumber / 10) % 10;
                    userNum3 = (userNumber % 10) % 10;

                    if (userNum1 == userNum2)
                    {
                        Console.WriteLine("다시 입력해 주세요");
                        continue;
                    }

                    if (userNum2 == userNum3)
                    {
                        Console.WriteLine("다시 입력해 주세요");
                        continue;
                    }

                    if (userNum1 == userNum3)
                    {
                        Console.WriteLine("다시 입력해 주세요");
                        continue;
                    }

                    isUserValidating = false;
                    // Console.WriteLine($"디버깅용 유저 값 : {userNum1}{userNum2}{userNum3}");
                }

                int strike = 0;
                int ball = 0;
                
                if (userNum1 == computerNum1)
                {
                    strike++;
                }
                if (userNum1 == computerNum2)
                {
                    ball++;
                }
                if (userNum1 == computerNum3)
                {
                    ball++;
                }
                if (userNum2 == computerNum1)
                {
                    ball++;
                }
                if (userNum2 == computerNum2)
                {
                    strike++;
                }
                if (userNum2 == computerNum3)
                {
                    ball++;
                }
                if (userNum3 == computerNum1)
                {
                    ball++;
                }
                if (userNum3 == computerNum2)
                {
                    ball++;
                }
                if (userNum3 == computerNum3)
                {
                    strike++;
                }
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine($"{userNumber}의 결과는!??!?");
                Console.ResetColor();
                bool isOut = (strike == 0 && ball == 0);
                if (isOut)
                {
                    Console.WriteLine("🔴🔴🔴 아웃!!!!!!");
                }
                else
                {
                    for (int j = 0; j < strike; j++)
                    {
                        Console.Write("⚾️");                        
                    }
                    Console.Write("스트라이크 & ");
                    
                    for (int k = 0; k < ball; k++)
                    {
                        Console.Write("⚾️");                        
                    }
                    Console.Write("볼\n\n");
                }
                
                if (strike == 3)
                {
                    isUserWin = true;
                    break;
                }
            }

            if (isUserWin)
            {
                Console.WriteLine("🎊축하합니다 당신이 이겼습니다🎊");
            }
            else
            {
                Console.WriteLine("컴퓨터의 승리입니다 😝 멍청한 인간 🤔🤔");
            }
        }
```