---
layout: post
title: 자료구조 & 알고리즘 1 - 리스트, 딕셔너리, 큐, 스택 / 심화문제 풀이
author: dduckchul
tags : [C#, 자료구조, 메타버스부트캠프]
---
# 문제1 - 예제 123 + 예제의 심화
## Item, UsableItem, Weapon, Player객체  

```cs
public class Example
{
    public class Item
    {
        private string _name;
        
        public Item(string name)
        {
            _name = name;
        }
        public string Name
        {
            get { return _name; }
            set { _name = value; }
        }        
    }
    
    public class UsableItem : Example.Item
    {
        private int _price;
        private int _quantity;

        public UsableItem(string name, int price, int quantity) : base(name)
        {
            _price = price;
            _quantity = quantity;
        }

        public int Price
        {
            get { return _price; }
            set { _price = value; }
        }

        public int Quantity
        {
            get { return _quantity; }
            set { _quantity = value; }
        }

        public int TotalPrice()
        {
            return Price * Quantity;
        }
    }

    public class Weapon : Example.Item
    {
        public Weapon(string name, int attackRate) : base(name)
        {
            _attackRate = attackRate;
        }
        
        private int _attackRate;

        public int AttackRate
        {
            get {return _attackRate;}
            set { _attackRate = value; }
        }

        public void WeaponEnforcement()
        {
            AttackRate *= 2;
        }
    }            
    
    public class Player<T>
    {
        private T[] _inv;
        private bool _isCustomer;
        private int _count;
        
        // 30개 넘 많으니까 5개로,, 무료유저는 5개만
        public const int FreeMaxCount = 5;
        
        public Player()
        {
            _inv = new T[FreeMaxCount];
        }
        
        public string Name
        {
            get;
            set;
        }

        public uint Money
        {
            get;
            set;
        }

        public T[] Inventory
        {
            get { return _inv; }
        }
        
        public int Count
        {
            get { return _count; }
            private set { _count = value; }
        }
        
        public T this [int i]
        {
            get { return _inv[i];}
            set { _inv[i] = value; }
        }

        public bool IsCustomer
        {
            get { return _isCustomer;}
            set {  _isCustomer = value; }
        }

        // 결제 태도가 좋은지 설정
        public void WhenPayMoney()
        {
            IsCustomer = true;
        }

        public void AddItem(T item)
        {
            // 결재 태도가 바르지 못한 친구들은 가세요라
            if(IsCustomer == false && Count >= FreeMaxCount)
            {
                Console.WriteLine("창고가 꽉 찼습니다. 결제하세요");
                return;
            }
            
            if (IsCustomer && Count >= _inv.Length)
            {
                Array.Resize(ref _inv, _inv.Length * 2);
            }
            
            Inventory[Count++] = item;
        }
        
        // 전체 출력
        public void ShowInventory()
        {
            foreach (T i in Inventory)
            {
                if (i is UsableItem)
                {
                    PrintUsableItem(i as UsableItem);
                }

                if (i is Weapon)
                {
                    PrintWeapon(i as Weapon);
                }
            }
        }

        // 1번이면 무기 2번이면 아이템 출력
        private void ShowInventory(Item targetItem, int choice)
        {
            if (targetItem is Weapon && choice == 1)
            {
                PrintWeapon(targetItem as Weapon);
            }

            if (targetItem is UsableItem && choice == 2)
            {
                PrintUsableItem(targetItem as UsableItem);
            }
        }
        
        public void UseMyItem()
        {
            bool printTitle = true;
            Console.WriteLine("무기 목록은 1번, 사용 가능 아이템은 2번을 눌러주세요");
            ConsoleKeyInfo keyInfo = Console.ReadKey();
            Console.WriteLine();

            foreach (T someThing in Inventory)
            {
                if (!(someThing is Item))
                {
                    continue;
                }

                Item item = someThing as Item;
                
                if(keyInfo.Key == ConsoleKey.D1 && printTitle)
                {
                    printTitle = false;
                    Console.WriteLine("아이템명\t공격력");                    
                }
                else if(keyInfo.Key == ConsoleKey.D2 && printTitle)
                {
                    printTitle = false;
                    Console.WriteLine("아이템명\t가격\t갯수\t합계");                        
                }
                ShowInventory(item, int.Parse(keyInfo.KeyChar.ToString()));
            }
        }
    }
    
    public static void PrintUsableItem(UsableItem ui)
    {
        Console.WriteLine($"{ui.Name}\t{ui.Price}\t{ui.Quantity}\t{ui.TotalPrice()}");
    }
    
    public static void PrintWeapon(Weapon w)
    {
        Console.WriteLine($"{w.Name}\t{w.AttackRate}");        
    }
} 
```
  
## Main.cs  
  
```cs
Example.Player<Example.Item> player = new Example.Player<Example.Item>();
player.WhenPayMoney();

for (int i = 0; i < 10; i++)
{
    Example.Item usable = new Example.UsableItem("좋은아이템"+i, i*100, i);
    Example.Item weapon = new Example.Weapon("좋은무기"+(10-i), (10-i)*100);
    player.AddItem(usable);
    player.AddItem(weapon);
}

player.ShowInventory();
player.UseMyItem();

// 무기 갖고와서 강화 & 확인 & 출력
if (player.Inventory[1] is Example.Weapon)
{
    Example.Weapon tempWeapon = player.Inventory[1] as Example.Weapon;
    tempWeapon?.WeaponEnforcement();
    tempWeapon?.WeaponEnforcement();
    Example.PrintWeapon(tempWeapon);
}
```

## 구현할때 생각한점..?
* Player의 인벤토리에 뭐가 들어갈지 모르니까, Player<T>로 생성하여 안에 T값을 인벤토리에 전달하도록 수정.
* ShowInventory를 다형성으로 구현해서 인자없이 넣으면 인벤토리 전체를 출력, 인자를 Item과 1,2번에 나눠서 받으면 해당 아이템이 UsableItem인지, Weapon인지 골라서 출력한다
  * ShowInventory에 출력하는 내용들을 제네릭에 따라 변경해 줘야하다보니, 중복되는 코드가 많다. 
  * 따라서 PrintUsableItem과 PrintWeapon을 static 메서드로 뽑아서 각각 UsableItem을 넘기거나 Weapon을 받아서 호출 할 수 있게 바꿨다.

---  

# 심화 문제 1
* Gun이라는 클래스와 Projectile이라는 클래스를 만들겠습니다. 
* 그리고 Projectile 클래스를 상속받는 Bullet클래스와 Grenade 클래스를 만듭니다. 
* Grenade와 Bullet클래스에는 각각 필드로 int _damage 하나만 담아 두고, Gun 클래스에서 Bullet들을 담을 수 있는 List, Grenade들을 담을 수 있는 List, 총 두개의 리스트를 맴버변수로 가지게 합니다.
  
## 1-2
* Gun의 생성자를 만들어서 해당 두 컨테이너들을 뉴할당 시켜줍니다. 
* 마지막으로, Gun에 컨테이너를 하나 더 추가하겠습니다. 
* 키값으로는 string을 받고, 내용으로는 방금 만들어진 List 컨테이너를 담는 Dictionary 필드를 만들어 주시기 바랍니다.
  
## 1-3
* Gun의 생성자를 수정합니다. 
* 방금 만들어진 딕셔너리를 뉴할당 하고, Bullet을 담은 List와 Grenade를 담은 List 둘을 각각 "Bullet", "Grenade" 이라는 키값으로 저장시켜주시기 바랍니다. 
* 이를 위해 리팩토링을 진행해주시길 바랍니다
  
## 1-4
* Gun에 메소드를 하나 더 추가하겠습니다. 
* "Bullet" 이라는 인자값을 넘겨주면 총알이 담긴 리스트를 반환 받고, "Grenade"라는 인자값을 넘겨주면 수류탄이 담긴 리스트를 반환 받는 메소드를 작성하여 주세요

## Gun.cs
```cs
public class Gun
{
    // 이것도 필요없잖아..
    // private List<Bullet> _bullets;
    // private List<Grenade> _grenade;
    private Dictionary<string, List<Projectile>> _dict; 
        
    public Gun()
    {
        List<Projectile> tempBullets = new List<Projectile>();
        List<Projectile> tempGrenade = new List<Projectile>();

        _dict = new Dictionary<string, List<Projectile>>
        {
            { "Bullet", tempBullets }, { "Grenade", tempGrenade }
        };

        // 형변환이 안되는디....
        // _bullets = (List<Bullet>)tempBullets;
        // _grenade = (List<Grenade>)tempGrenade;
    }

    public List<Bullet> GetBullet()
    {
        List<Bullet> tempBullets = new List<Bullet>();
        List<Projectile> tempDictValue = _dict["Bullet"];
        foreach (Projectile p in tempDictValue)
        {
            if (p is Bullet)
            {
                tempBullets.Add((Bullet)p);                    
            }
        }
        return tempBullets;
    }
    
    public List<Grenade> GetGrenade()
    {
        List<Grenade> tempGrenades = new List<Grenade>();
        List<Projectile> tempDictValue = _dict["Grenade"];
        foreach (Projectile p in tempDictValue)
        {
            if (p is Grenade)
            {
                tempGrenades.Add((Grenade)p);                    
            }
        }            
        return tempGrenades;
    }
    
    // ConvertAll 이용한 깔끔 버전?
    // 문제, 실수로 dict에 bullet에 Grenade를 넣으면 타입 캐스트 익셉션이 발생, 해결 할수가 있나?
    public List<Bullet> GetBulletV2()
    {
        List<Projectile> tempDictValue = _dict["Bullet"];
        List<Bullet> bullets = tempDictValue.ConvertAll(ConvertToDerived<Bullet>);
        return bullets;
    }
    
    public List<Grenade> GetGrenadesV2()
    {
        List<Projectile> tempDictValue = _dict["Grenade"];
        List<Grenade> grenades = tempDictValue.ConvertAll(ConvertToDerived<Grenade>);
        return grenades;
    }
    
    public static Projectile ConvertToParent<T>(T someProj)
    {
        if (someProj is Bullet)
        {
            return (Projectile)(someProj as Bullet);
        }

        if (someProj is Grenade)
        {
            return (Projectile)(someProj as Grenade);
        }

        // 없으면 널
        return null;
    }        
    
    // https://learn.microsoft.com/ko-kr/dotnet/api/system.converter-2?view=net-8.0
    // https://learn.microsoft.com/ko-kr/dotnet/csharp/programming-guide/generics/generic-methods
    private static T ConvertToDerived<T> (Projectile someProj) where T : Projectile
    {
        if (someProj is Bullet || someProj is Grenade)
        {
            return (T)someProj;
        }
        // 없으면 널
        return null;
    }        

    public List<Projectile> GetProjectileListByKey(string key)
    {
        if (_dict.ContainsKey(key))
        {
            return _dict[key];
        }

        Console.WriteLine("해당하는 키가 없습니다");
        return null;
    }
}

// 발사체
public abstract class Projectile
{
    protected int _damage;
    public abstract int Damage
    {
        get;
        set;
    }
    public abstract void PrintDamage();
}
public class Bullet : Projectile
{
    public override int Damage
    {
        get { return _damage; }
        set { _damage = value;  }
    }

    public override void PrintDamage()
    {
        Console.WriteLine("총알 데미지 : "+ Damage);
    }
}

public class Grenade : Projectile
{
    public override int Damage
    {
        get { return _damage; }
        set { _damage = value;  }
    }
    public override void PrintDamage()
    {
        Console.WriteLine("수류탄 데미지 : "+ Damage);
    }        
}
```
## Main.cs
```cs
Gun gun = new Gun();
// 예제 쉽게 만들기 위해 추가
List<Projectile> bulletsList = gun.GetProjectileListByKey("Bullet");
List<Projectile> grenadeList = gun.GetProjectileListByKey("Grenade");

bulletsList.Add(new Bullet{Damage = 20});
bulletsList.Add(new Bullet{Damage = 30});
bulletsList.Add(new Bullet{Damage = 40});

grenadeList.Add(new Grenade{Damage = 10});
grenadeList.Add(new Grenade{Damage = 30});
grenadeList.Add(new Grenade{Damage = 50});

foreach (var b in gun.GetBullet())
{
    b.PrintDamage();
}

foreach (var b2 in gun.GetBulletV2())
{
    b2.PrintDamage();
}

foreach (var g2 in gun.GetGrenadesV2())
{
    g2.PrintDamage();
}
```
## 구현하면서 생각해본 내용들
* Projectile -> 발사체를 추상 클래스로 만들고, 추상 프로퍼티, 프린트 메소드를 추상 메소드로 선언
* 형변환 코드를 수동으로 구현 -> 리스트에 담기는것은 자동으로 다운캐스팅 / 업캐스팅이 안된다.
    * 좀 더 찾아보니 ConvertAll 이라는 메서드가 리스트에 구현되어있어, 그걸 쓰는것으로 구현함.
    * v2 버전으로 바꾸고 난 후? 깔끔은 한데 이런식으로 쓰는지? -> 선생님의 답변은 ConvertAll는 코스트가 많이 든다.
        * (아마도 새 배열을 만들고 복사하는것 외에도 연산이 추가적으로 있으니까?)
        * 게임 쪽에서는 v1 버전으로 하는것을 추천

# 심화 2 Queue 활용
  * Milk라는 클래스와 VendingMachine 이라는 클래스를 하나 만들겠습니다. 
  * Milk의 맴버변수로, 유통기한을 나타내는 int를 작성해주시기 바랍니다.
  * VendingMachine 클래스에는 Queue를 활용하여 Milk를 담을 수 있는 컨테이너를 필드로 작성하여 주시기 바랍니다. 
  * 벤딩머신의 메소드로, 우유를 집어넣는 코드와 우유를 꺼내는 기능을 작성하되, 꺼낼때는 콘솔에 유통기한 및 큐에 남아있는 갯수를 출력하는 기능을 작성해주시기 바랍니다. 
  * 갯수가 0일때 우유를 꺼내는 기능을 호출하게 되면 꺼내는 대신 다른 멘트가 나오게끔 작성해주시길 바랍니다.

## Milk.cs
```cs
public class Milk
{
    public int ExpiredDate
    {
        get;
        set;
    }

    public void ShowExpiredDate()
    {
        Console.WriteLine("우유의 유통기한 : " + ExpiredDate + "일");
    }
}

public class VendingMachine
{
    private Queue<Milk> _milks;

    public Queue<Milk> Milks
    {
        get { return _milks; }
    }

    public VendingMachine()
    {
        _milks = new Queue<Milk>();
    }

    public void InsertMilk(Milk milk)
    {
        _milks.Enqueue(milk);
    }

    public Milk GetMilk()
    {
        if (_milks.Count == 0)
        {
            Console.WriteLine("우유가 다 떨어졌습니다");
            return null;
        }
        
        Milk mymilk = _milks.Dequeue();
        mymilk.ShowExpiredDate();
        Console.WriteLine("===남은 우유 갯수 : " + _milks.Count + "===");
        
        return mymilk;
    }
}
```
## Main.cs
```cs
// 심화 2
VendingMachine vm = new VendingMachine();
// 밀크 없을때 출력 여부
Milk m0 = vm.GetMilk();

// 널은 출력이 안되나봄.. 다행인가!?
Console.WriteLine(m0);

for (int i = 0; i < 5; i++)
{
    vm.InsertMilk(new Milk{ExpiredDate = i});
}

Milk m1 = vm.GetMilk();

// 그냥 남은 우유 출력해보기
foreach (var m in vm.Milks)
{
    m.ShowExpiredDate();
    // vm.GetMilk(); 요거는 출력 하는 중에 빠지기 때문에 안됨
    // foreach 가 돌고있는 을 수정하려고 하면 불변 객체이기 때문에 안된다고함.
}

while (vm.GetMilk() != null)
{
    // Q1. 다 떨어질때까지는 이런식으로 해야되나?
}
```
  
## 질문사항 정리  
* forEach에서는? in 이 돌때 안의 대상값을 const처럼 바꾼다. // 익셉션도 컬렉션이 수정되었습니다 형태로 나옴
* for 에서는..? 먼가 다른 이유때문에 터지겠지? 렝스가 고정일텐데.. 수정한다거나? 먼가 잘못짠다거나
* 큐나 스택에서 while을 돌려서 겟 밀크 하는것처럼 짜는것은?
  - 굳이 넣고 빼는것을 제한한건 이유가 있어서일텐데, 자료 구조형을 맞게 썼는지 먼저 판단이 필요할것같다.

# 심화 3 플레잉 카드 / 자료구조 구현

* Shape라는 Enum을 만들어서 Spade, Heart, Clover, Diamond 네 가지를 요소로 가지게 합니다.
* Card라는 클래스를 만듭니다. 
* 위 열거형을 Card클래스 맴버변수로 가지게 해주세요. 
* Card클래스에는 숫자를 나타내는 int형을 넣되, a는 1, J는 11, Q는 12, K는 13이라고 가정하겠습니다.

* CardDeck이라는 클래스도 하나 제작하도록 하겠습니다. 
* Card 객체를 52개 담을 수 있는 ???(자료구조) 들을 활용하여 두 개 만드시되, 하나는 unusedCards, 하나는 usedCards 의 이름으로 만들겠습니다. 

* CardDeck 클래스의 생성자에 unusedCards 속 52개의 Card를 중복없이 뉴할당해서 넣어주는 코드를 작성하여 주시기 바랍니다. 
* 카드를 섞는 메소드를 작성하거나 생성자에서 바로 진행해주시기 바랍니다.

* CardDeck 클래스에 아직 쓰지 않은 맨 위 카드를 보기만 하는 ShowTopCard 기능을 제작하여 주시기 바랍니다. 
* 이는 맨 상단의 카드를 보기만 할 뿐 카드를 소모하진 않습니다. DrawCard라는 메소드도 하나 제작하겠습니다. 
* 이 메소드는 아직 쓰지 않은 덱의 맨 위 카드를 반환받고 그와 동시에 usedCard에 쌓는 기능을 수행합니다.

## Card.cs
```cs
    public enum Shape{Spade, Heart, Clover, Diamond, End}
    public enum CharNumber{A=1, J=11, Q, K, End}
    public class Card
    {
        private Shape _shape;
        private int _number;

        public Shape Shape
        {
            get { return _shape; }
        }

        public int Number
        {
            get { return _number; }
        }

        public Card(Shape shape, int number)
        {
            _shape = shape;
            _number = number;
        }
        
        public void PrintCard()
        {
            CharNumber charNumber;
            bool parsedNum = Enum.TryParse(Number.ToString(), out charNumber);
            Console.WriteLine($"Shape:{Shape}\tNumber:{(parsedNum ? charNumber.ToString() : Number.ToString())}");
        }        
    }

    public class CardDeck
    {
        public const int MaxCardNum = 52;
        private Stack<Card> _unusedCards;
        private Stack<Card> _usedCard;

        public CardDeck()
        {
            _unusedCards = new Stack<Card>();
            _usedCard = new Stack<Card>();
        }

        public Stack<Card> UnusedCards
        {
            get { return _unusedCards; }
        }
        
        public Stack<Card> UsedCards
        {
            get { return _usedCard; }
        }

        public List<Card> MakeInitDeck()
        {
            List<Card> initDeck = new List<Card>();
            for (int i = 0; i < (int)Shape.End; i++)
            {
                for (int j = 1; j < (int)CharNumber.End; j++)
                {
                    initDeck.Add(new Card((Shape)i, j));
                }
            }

            return initDeck;
        }
        
        public void ShuffleToStack(List<Card> initDeck)
        {
            Random random = new Random();
            while (initDeck.Count > 0)
            {
                // 랜덤으로 카운트에 있는 인덱스 카드를 찾아서 스택으로 푸쉬해버림
                int randomCard = random.Next(initDeck.Count);
                Card tempCard = initDeck[randomCard];
                initDeck.RemoveAt(randomCard);
                UnusedCards.Push(tempCard);
            }
        }

        public Card ShowTopCard()
        {
            return UnusedCards.Peek();
        }

        public Card DrawCard()
        {
            Card drawCard = UnusedCards.Pop();
            UsedCards.Push(drawCard);
            return drawCard;
        }
    }
```

## Main.cs
```cs
// 심화 3
CardDeck cardDeck = new CardDeck();
cardDeck.ShuffleToStack(cardDeck.MakeInitDeck());

// 잘 됐는지 테스트
cardDeck.ShowTopCard().PrintCard(); // 맨 윗장 peek
cardDeck.DrawCard().PrintCard(); // 맨 윗장 pop 후 버리는 stack에 push
cardDeck.UnusedCards.Peek().PrintCard(); // 다음장 카드 확인
cardDeck.UsedCards.Peek().PrintCard(); // 버린 stack의 카드 확인
```

* 마작 구현하면서 해본것들이라 어렵지는 않았지만 자료구조형 리팩토링 할때 참고해서 쓰면 좋을듯?
* 셔플은 그냥 대충 구현함.. 성능으로 따지면 Object를 쓰지않고 배열 쓰는 마작버전이 더 좋을듯

```cs
// Fisher–Yates shuffle
// https://stackoverflow.com/questions/108819/best-way-to-randomize-an-array-with-net
public static void ShuffleDeck(Tiles.Tile[] tiles)
{
    int n = tiles.Length;
    Random random = new Random();
    while (n > 1)
    {
        int k = random.Next(n--);
        Tiles.Tile temp = tiles[n];
        tiles[n] = tiles[k];
        tiles[k] = temp;
    }
}
```
