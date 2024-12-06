---
layout: post
title: 구조체 - 심화 문제 풀어보기
subtitle: C# 구조체와 객체 타입 되짚어보기
author: dduckchul
tags : [C#, Java, 구조체, 메타버스부트캠프]
---
# 구조체 - 심화 과제
- Weapon 이라는 구조체 설계도를 하나 만든 후, 속성으로는 public 문자열 형 name을 가지게 하세요.
- Soilder이라는 구조체 설계도를 하나 더 만든 후, 속성으로 Weapon구조체를 담을 수 있는 배열 및 현재 손에 들고 있는 Weapon이 무엇인지 저장할 배열의 인덱스로 사용할 정수형 변수 하나를 작성합니다.
- 메인에 돌아와서 솔져 구조체를 하나 만들어줍니다. 솔져의 무기 목록 배열에 Weapon 구조체를 3개 담을 수 있게 초기화 시켜주시고, 솔져가 가진 무기의 배열에 본인이 희망하는 이름을 담은 무기 3가지를 모두 기입하여 줍니다. 처음엔 1번을 들고 있습니다(인간의 말로)
- 함수를 하나 만들되, 솔져형을 참조형 인자로 받는 void형 함수를 제작할 것입니다. 함수의 이름은 ChangeWeapon으로 하고 기능은 다음과 같습니다.
    - 함수가 호출되면 배열 속의 Weapon 이름들을 전부 출력하고 다음줄에 어떤 무기로 바꿀것인지 콘솔에 적습니다.
    - 유저로부터 1~3사이 정수를 입력받되 1~3이 아닐 경우, 제대로 1~3사이 정수를 입력하라고 콘솔에 입력 후 무한으로 재입력을 요구합니다.
    - 제대로 1,2,3 중 하나를 입력 받았을 경우, 만약 본인의 손에 들고 있는 무기와 같은 무기를 입력하였어도 '현재 들고 있는 무기와 동일합니다' 를 출력 후, 재입력을 요구합니다.
    - 본인 손에 들고 있는 무기와는 다른 무기를 정상적으로 골랐다면 콘솔 출력을 통해 해당 무기로 바뀌었음을 출력합니다.
    - 이후 메인 문에 ChangeWeapon 함수를 한줄 한줄 3번 실행하되, 아래 각각의 상황에 맞게 호출하세요.
        - 시작 시 본인이 들고 있는 무기와 일치하는 숫자를 유저가 입력한 상황.
        - 1~3이 아닌 숫자를 입력한 상황.
        - 정상적으로 무기 변경이 일어날 수 있는 상황.
- 이외 자유롭게 제작, 자유롭게 테스트

## 짜긴 짰는데... 뭔가 이상한데..?
### 예제 클래스
```cs
public class DeepExample
{
    public struct Weapon
    {
        public string name;
    }

    public struct Soldier
    {
        public Weapon[] weapons;
        public int currentWeapon;
    }

    public void ChangeWeapon(Soldier soldier)
    {
        PrintCurrentWeapon(soldier);
        Console.Write("바꿀 무기를 입력해주세요 : ");
        
        ConsoleKeyInfo keyInfo = Console.ReadKey();
        int parsedKey = 0;
        bool isParsed = int.TryParse(keyInfo.KeyChar.ToString(), out parsedKey);
        
        // 파싱 된거에서 인덱스를 구하기위해 편하게 1빼줌
        int parsedWeaponIndex = parsedKey - 1;
        bool isOutOfWeaponIndex = parsedKey < 1 || parsedKey > 3;

        Console.Clear();
        // int 로 변환되었거나, 1미만 3초과의 정수가 들어왔을 경우
        if (isParsed == false || isOutOfWeaponIndex)
        {
            Console.WriteLine("1~3사이 숫자를 입력해주세요");
        } else if (parsedWeaponIndex == soldier.currentWeapon)
        {
            Console.WriteLine("현재 들고있는 무기와 동일합니다");
        } else
        {
            soldier.currentWeapon = parsedWeaponIndex;
            Console.WriteLine($"현재 무기가 {soldier.weapons[soldier.currentWeapon].name}로 바뀜");                
        }
    }

    public void PrintCurrentWeapon(Soldier soldier)
    {
        // 보기 귀찮아서 변수 선언
        int weaponIndex = soldier.currentWeapon;
        Weapon[] currentWeapons = soldier.weapons;
        
        // soilder 의 무기 목록 전부 출력
        for (int i = 0; i < currentWeapons.Length; i++)
        {
            Console.Write($"|\t{i+1}번슬롯\t|");
        }            
        Console.WriteLine();
        foreach (Weapon w in currentWeapons)
        {
            Console.Write("|\t" + w.name + "\t|");
        }
        Console.WriteLine("\n---------------------------------\n");
        Console.WriteLine($"현재 무기 : {currentWeapons[weaponIndex].name}");            
    }
}
```

### 메인 클래스
```cs
public static void Main(string[] args)
{
    Console.WriteLine("------무기 스왑해보기------");         
    DeepExample deepExample = new DeepExample();
    DeepExample.Weapon baseWeapon;
    DeepExample.Weapon primaryWeapon;
    DeepExample.Weapon secondaryWeapon;

    // 초기화
    Console.Clear();
    baseWeapon.name = "주먹";
    primaryWeapon.name = "AK47";
    secondaryWeapon.name = "구르카";

    DeepExample.Soldier me;
    me.weapons = new DeepExample.Weapon[3]
    {
        baseWeapon, secondaryWeapon, primaryWeapon
    };
    me.currentWeapon = 0;
    deepExample.ChangeWeapon(me);
    deepExample.ChangeWeapon(me);
    deepExample.ChangeWeapon(me);
    // 근데 me.currentWeapon이 안바뀐다?!
}
```
* 실제 코드를 main에서 수행했을때, 구조체로 넘긴 me의 변수값이 바뀌지를 않는다..  

![뭐임??](https://i.giphy.com/media/v1.Y2lkPTc5MGI3NjExeDh5cTZlYTg1OGZmZG51Z3F4eGFqcjJuZmF0bGFhdTI3MGR3ZGNsbiZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9dg/A19tglYyfc7JPQYwNi/giphy.gif){: .mx-auto.d-block :}

------------------------ 

## 뭐임!?
요구사항이 살짝 수정되었다고 하셔서 다시 확인해보니..  

{: .box-note}
함수를 하나 만들되, 솔져형을 **참조형 인자**로 받는 void형 함수를 제작할 것입니다.

### DeepExample.cs
``` cs
    // 펑션 참조값으로 바꾸기
    public void ChangeWeapon(ref Soldier soldier){}
```
### Program.cs
``` cs
    // 호출부도 역시 바꾸기
    deepExample.ChangeWeapon(ref me);
```

함수와 호출부를 모두 참조값을 보도록 바꿔주니 해결!  
C#에는 객체와 달리 구조체가 따로 있는데 뭐가 또 다른게 있을까..?

------------------------  

## C#에서 구조체와 클래스 간의 차이
아래는 책에서 발췌한 표이다.  

| 특징 | 클래스 | 구조체 |
| :------ |:--- | :--- |
| 키워드 | class | struct |
| 형식 | 참조 형식 (힙에 할당) | 값 형식 (스택에 할당) |
| 복사 | 얕은 복사 (shallow copy) | 깊은 복사 (Deep copy) |
| 인스턴스 생성 | new 연산자와 생성자 필요 | 선언만으로도 생성 |
| 생성자 | 매개 변수 없는 생성자 선언 가능 | 매개변수 없는 생성자 선언 불가능 | 
| 상속 | 가능 | 값 형식이므로 불가능 |

* 값 형식과 참조 형식의 가장 큰 차이는..? Heap에 할당되느냐, stack에 할당 되느냐..
  * 구조체의 인스턴스는 스택에 할당, 객체의 인스턴스는 힙 메모리에 할당
  * 스택은 스택이 날라가면서 다 날라가는데, 객체는 가비지 컬렉팅이 될때까지 버려지지 않는다..!
  * 이게 가장 큰 구조체와 오브젝트의 차이 
  * C#이 게임에서도 쓰는 이유가 여기에 있지 않을까..? 라는 생각..

------------------------

## 와 근데 이런것도 다르네!?
- 나와 비슷한 고민 : 자바 개발자로서 c# 헷갈리는점 [(링크)](https://stackoverflow.com/questions/9251608/are-structs-pass-by-value)  

### 요약하자면...
- ref 키워드나 out 키워드를 쓰지 않는 한, **C#의 메서드 에서는 모든게 PassByValue 이다**

```cs  
struct PointStruct { 
    public int x, y;
    public PointStruct(int i, int i1)
    {
        x = i;
        y = i1;
    }
}
class PointClass { 
    public int x, y;
    public PointClass(int i, int i1)
    {
        x = i;
        y = i1;
    }
}

public static void main(string args) {
    var pc = new PointClass(1, 1);
    var ps = new PointStruct(1, 1);

    ExampleMethod(pc, ps);
    Console.WriteLine("pc :" + pc.x + ", " + pc.y);
    Console.WriteLine("ps :" + ps.x + ", " + ps.y);
}


static void ExampleMethod(PointClass apc, PointStruct aps){
    apc = new PointClass(2, 2);
    aps = new PointStruct(2, 2);
}
```  

* 위 코드 실행 이후에 pc와 ps의 결과는..?

{: .box-note}
Output of pc and ps after calling the method:  
pc: {x: 1, y: 1}  
ps: {x: 1, y: 1}  

구조체는 값 복사 된다고 이해해서 변하지 않는건 알겠는데.. 클래스도 대입이 안된다고!?  

* 선생님이 그려주신 그림
![그림](/assets/img/posting/2024/everythingispassbyvalue.excalidraw.svg)

* example method 의 클래스에 apc로 생성한 클래스를 넣는게 의도라면  
완전히 쓸데없는 코드가 된 것. (게다가 apc 가 가르키는 객체는 가비지 컬렉팅 돌기 전에는 계속 남아있다.)
* 결국 pc와 ps는 처음 값만 들어갈 수 밖에 없다.
* 자바는 주소값을 다시 저기에 할당해줄텐데 동일한 코드로 다른 결과가 나온다..!
