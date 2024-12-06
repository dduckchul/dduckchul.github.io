---
layout: post
title: 가붕이 시뮬레이터 만들기
author: dduckchul
tags : [C#, 배열, 메타버스부트캠프]
---
# 턴제 스킬 쿨 동작 프로그램 만들기

* 플레이어에게 4개의 스킬이 있고, 각각의 스킬명이 존재합니다. 초기값은 다음과 같습니다. 변수를 쓰셔도, 배열을 쓰셔도 상관 없습니다  

| 스킬 | 쿨타임 |
| :-- | :-- |
| Q스킬 | 5 |
| W스킬 | 3 |
| E스킬 | 10 |
| R스킬 | 7 |

* 콘솔을 킨 첫 화면에서 일단 모든 스킬과 턴을 보여줍니다.
* 그 아래에 유저에게 선택권을 줍니다.

1. 턴을 넘길지, 스킬을 사용할 지 두 선택지를 줍니다.
2. 스킬을 선택하였다면, 어느 스킬을 사용할 지 입력받고,  
만약 그 스킬의 쿨타임이 0이면 다시 쿨타임 초기 숫자로 원복(Q는 5, W는3, E는 10, R은 7)    
  - 쿨타임이 돌아오지 않은 것을 선택 시, 쿨타임이 돌아오지 않았다는 메시지 출력
3. 턴 넘김을 사용 시, 모든 스킬 쿨타임 1 감소.  
만약 쿨타임 0이고 스킬을 사용하지 않는다면 0으로 유지.  
만약 쿨타임이 0이라면 0을 출력하는 것이 아닌, “스킬준비완료” 를 출력합니다
4. 원하는 대로 커스터마이징 해도 괜찮습니다.
5. 숫자 0을 입력 할때 까지 이를 무한으로 반복합니다;

## 의사코드 짜보기
1. QWER 스킬의 모음을 이중 포문으로 보여준다.  
  * 이중포문으로 출력시, 중간에 스킬과 : 는 0번에만 넣어준다.
  * 쿨타임이 0 이라면 0 을 출력하는 것이 아니라 "스킬 준비 완료"를 출력한다.  
2. 유저에게 입력가능한 선택창을 준다.  
  * 0번 종료, 1번 턴넘김, 2번 스킬사용
  * 2번 스킬 사용은 QWER로 판단한다.
3. 턴넘김을 사용했다면 모든 스킬의 쿨타임을 1초씩 감소시킨다.
  * 0초 이하일경우 다시 0으로 설정해준다
4. 쿨타임이 0이고 스킬을 사용하지 않는다면 0으로 계속 유지한다.
5. 스킬을 사용한다면 선택한 스킬을 출력하고, 쿨다운을 증가시킨다
  * 쿨타임이 0보다 큰 스킬은 쿨타임이 돌아오지 않았습니다 출력 후 초기로 돌아간다.
```cs
public void turnBasedActionGame()
{
    Console.Clear();
    // 스킬 설정 및 초기화
    string[,] skillCoolDowns = new string[4, 4]
    {
        { "Q", "5", "5","돌격!!!!\n" },
        { "W", "3", "3","확고한 의지로!\n" },
        { "E", "10", "10","눈도 깜짝 안한다!\n" },
        { "R", "7", "7","🗡️데마시아!!!!!!🗡️\n" }
    };            
    while (true)
    {
        Console.WriteLine("🤺가렌 시뮬레이터🤺");
        Console.WriteLine("사용 가능한 스킬은 다음과 같습니다\n");
        for (int i = 0; i < skillCoolDowns.GetLength(0); i++)
        {
            for (int j = 0; j < 2; j++)
            {
                Console.Write($"{skillCoolDowns[i,j]}");
                if (j == 0)
                {
                    Console.Write(" 스킬 : ");
                }
                else if (j == 1 && skillCoolDowns[i,j] == "0")
                {
                    Console.Write("\b: 스킬 준비 완료");
                }
            }
            Console.WriteLine("");
        }
        Console.WriteLine("\n🕹🕹🕹🕹🕹🕹🕹🕹🕹🕹️🕹🕹🕹🕹🕹🕹🕹");
        Console.WriteLine("🕹\t액션을 선택해 주세요\t 🕹");
        Console.WriteLine("🕹🕹🕹🕹🕹🕹🕹🕹🕹️🕹🕹🕹🕹🕹🕹🕹🕹\n");
        Console.WriteLine("0. 종료");
        Console.WriteLine("1. 턴 넘김");
        Console.WriteLine("2. 스킬 사용");
        
        ConsoleKeyInfo keyInfo = Console.ReadKey(true);
        Console.Clear();                
        if (keyInfo.Key == ConsoleKey.D0)
        {
            Console.WriteLine("종료 합니다");
            break;
        }

        if (keyInfo.Key == ConsoleKey.D1)
        {
            for (int i = 0; i < skillCoolDowns.GetLength(0); i++)
            {
                int coolDown = int.Parse(skillCoolDowns[i,1]);
                coolDown--;
                if (coolDown <= 0)
                {
                    coolDown = 0;
                }
                // 아직 안배웠지만 coolDown.ToString(); 으로 해서 변환해도 될듯.
                skillCoolDowns[i, 1] = $"{coolDown}";
            }                    
        }

        if (keyInfo.Key == ConsoleKey.D2)
        {
            Console.Clear();
            Console.WriteLine("QWER 중 스킬을 눌러주세요");
            ConsoleKeyInfo skillKey = Console.ReadKey(true);

            int someSkillCoolDown = -1;
            int keyIndex = -1;
            if (skillKey.Key == ConsoleKey.Q)
            {
                string qSkillCoolDown = skillCoolDowns[0, 1];
                someSkillCoolDown = int.Parse(qSkillCoolDown);
                keyIndex = 0;
            }
            else if (skillKey.Key == ConsoleKey.W)
            {
                string qSkillCoolDown = skillCoolDowns[1, 1];
                someSkillCoolDown = int.Parse(qSkillCoolDown);
                keyIndex = 1;
            }
            else if (skillKey.Key == ConsoleKey.E)
            {
                string qSkillCoolDown = skillCoolDowns[2, 1];
                someSkillCoolDown = int.Parse(qSkillCoolDown);
                keyIndex = 2;                        
            }
            else if (skillKey.Key == ConsoleKey.R)
            {
                string qSkillCoolDown = skillCoolDowns[3, 1];
                someSkillCoolDown = int.Parse(qSkillCoolDown);
                keyIndex = 3;
            }
            else
            {
                Console.WriteLine("잘못된 키 입니다");
            }

            if (someSkillCoolDown > 0)
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("쿨타임이 돌아오지 않았습니다");
                Console.ResetColor();
            }
            else if(someSkillCoolDown == 0)
            {
                Console.WriteLine(skillCoolDowns[keyIndex,3]);
                skillCoolDowns[keyIndex, 1] = skillCoolDowns[keyIndex, 2];
            }
        }
    }
}
```

## 짜면서 더 추가로 구현해본것들
* 배열을 4x2로 하지 않고, QWER에 따른 몇개 데이터들을 더 얻기 편하게 하기위해,
  쿨다운 기본 초를 저장해 놓는 숫자와, 스킬에 따른 텍스트를 저장해 두었다
  * 쿨다운 계산시에 이중 포문으로 돌리지 않고 쿨다운을 저장할 수 있는 인덱스를 알고 있으니 한번만 돌린다.
  * 쿨다운 초기화시 저장해둔 스킬의 기본 시간을 이용한다.
  * 스킬 발동시에 대사를 출력한다.
* 2번을 눌렀을때 스킬 쿨다운과 눌렀던 키의 인덱스를 저장할 수 있는 임시 변수를 만들었다.
  * -1로 초기화

## 보완해야 할 점..?
* string [][] 이중배열로 변수를 선언 했기 때문에 쿨타임 계산할때 string -> int -> string 을 반복해서 변환해 주는 과정이 발생
  * 같이 담아 줄 수 있지 않을까?
* 맥 콘솔에서는 이모지가 정상적으로 노출 되나, 윈도우에서는 ??로 깨진다
  * [해결법?](https://stackoverflow.com/questions/67508469/how-to-show-emoji-in-c-sharp-console-output)
  * 일반적인 윈도우 10 환경에서는 해결하기가 어려울 것 같다. (마이크로소프트 마켓에서 windows terminal 앱 다운, 혹은 윈도우 11 사용)

* [FlowChart](https://viewer.diagrams.net/?tags=%7B%7D&lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1&title=turnbasedgame.drawio#Uhttps%3A%2F%2Fraw.githubusercontent.com%2Fdduckchul%2Fdduckchul.github.io%2Fgh-pages%2Fdocs%2Fassets%2Fimg%2Fturnbasedgame.drawio)