---
layout: post
title: 자료구조 & 알고리즘 2 - LinkedList 구현 심화
author: dduckchul
tags : [C#, 자료구조, 메타버스부트캠프]
---
# 심화과제
* LinkedList 수동 구현 과제를 리팩토링 혹은 기능을 추가하여 양방향 연결 리스트 기능을 수행 가능하게끔 만들어 주세요
* 코딩 처음에 배울때 C로 만들라고 했을때 눈물을 엉엉 흘리면서 이해 하나도 못했던 PTSD가... 으윽...

## 구현해보는 방법
  * 최대한 C#의 기본 링크드 리스트와 비슷하게 만들어본다.
  * Clear()
    * 링크드 리스트 내 요소들을 삭제 -> 참조 끊어버리면 알아서 제거될 거기 때문에, 제일쉬움
  * AnotherNode<T> FindAnotherNode(T value)
    * 안에 들어간 자료를 검색해서 맞으면 반환한다, equal 비교로 구현함. equal 비교로 구현하면 오브젝트도 아마 잘 나올거같긴함..?
  * void AddFirst(AnotherNode<T> value)
    * 앞부분부터 넣는다 / 예외사항은 없을때나 하나만 있을때
  * void AddLast(AnotherNode<T> value)
    * 뒷부분부터 넣는다 / 예외사항은 First랑 비슷
  * bool AddAfter(AnotherNode<T> value, T data)
    * T데이터를 특정 노드 뒤에 넣는다 / 케이스로 정리, 노드 못찾으면 안넣음
  * bool AddBefore(AnotherNode<T> value, T Data)
    * T데이터를 특정 노드 앞에 놓는다 / 케이스로 정리, 노드 못찾으면 안넣음
  * bool Remove(AnotherNode<T>)
    * 특정 노드를 찾아서 지운다, 노드 못찾으면 안지움
  * 나머지는 크게 구현하지 않아도 될거같아서.. 패스!

## 구현하다가 머리가 너무 아파서 케이스를 그림으로 그렸다..
  * AddBefore, AddAfter가 너무 머리아픔. Remove도 정리하는게 나을거 같아서 추가
  * 그림에 써져있는대로 구현하니까 그래도 뭔가 정리되는 느낌!  

![링크드 리스트 로직](/assets/img/posting/2024/linkedList.png)

## AnotherLinkedList.cs
```cs
    public class AnotherNode<T>
    {
        private T _data;
        protected AnotherNode<T> _next;
        protected AnotherNode<T> _prev;

        public AnotherNode(T data)
        {
            _data = data;
        }
        
        public T Data
        {
            get { return _data; }
            set { _data = value; }
        }
        
        public AnotherNode<T> Next
        {
            get { return _next;}
            set { _next = value; }
        }

        public AnotherNode<T> Prev
        {
            get { return _prev; }
            set { _prev = value; }
        }
    }
    
    public class AnotherLinkedList<T>
    {
        private int _count;
        private AnotherNode<T> _head;
        private AnotherNode<T> _tail;

        public int Count
        {
            get { return _count; }
            private set { _count = value; }
        }
        
        public AnotherNode<T> Head
        {
            get { return _head; }
            private set { _head = value; }
        }

        public AnotherNode<T> Tail
        {
            get { return _tail; }
            private set { _tail = value; }
        }

        public void AddFirst(AnotherNode<T> value)
        {
            // 처음 들어왔을때
            if (Head == null && Tail == null)
            {
                Head = value;
                Tail = value;
            }
            // 지금 한개만 있을때랑
            else if (Head == Tail)
            {
                value.Next = Head;
                Head.Prev = value;
                Tail = Head;
                Head = value;                
            }
            // 나머지는 계속 헤드에서 붙여넣기            
            else 
            {
                value.Next = Head;
                Head.Prev = value;
                Head = value;
            }

            Count++;
        }

        public void AddLast(AnotherNode<T> value)
        {
            if (Head == null && Tail == null)
            {
                Head = value;
                Tail = value;
            }
            else if (Head == Tail)
            {
                value.Prev = Tail;
                Tail.Next = value;
                Head = Tail;
                Tail = value;                
            }
            else 
            {
                value.Prev = Tail;
                Tail.Next = value;
                Tail = value;
            }

            Count++;
        }

        public bool AddAfter(AnotherNode<T> value, T data)
        {
            if (value == null)
            {
                Console.WriteLine("찾는 대상이 Null 입니다.");
            }

            AnotherNode<T> temp = Head;

            // 템프 끝까지 순회
            while (temp != value && temp != null)
            {
                temp = temp.Next;
            }
            
            if (temp == null)
            {
                Console.WriteLine("찾는 대상이 없습니다");
                return false;
            }
            
            // 조건 찾아서 인서트
            AnotherNode<T> newNode = new AnotherNode<T>(data);
            
            // 뒤로 넣을때 찾는 값이 꼬리였다면 꼬리에 넣기
            if (temp == Tail)
            {
                Tail.Next = newNode;
                newNode.Prev = Tail;
                Tail = newNode;
            }
            else
            {
                newNode.Prev = temp;
                newNode.Next = temp.Next;
                temp.Next.Prev = newNode;
                temp.Next = newNode;                
            }
            
            Count++;
            return true;
        }

        public bool AddBefore(AnotherNode<T> value, T data)
        {
            if (value == null)
            {
                Console.WriteLine("찾는 대상이 Null 입니다.");
            }

            AnotherNode<T> temp = Head;

            // 템프 끝까지 순회
            while (temp != value && temp != null)
            {
                temp = temp.Next;
            }
            
            if (temp == null)
            {
                Console.WriteLine("찾는 대상이 없습니다");
                return false;
            }
            
            // 조건 찾아서 인서트
            AnotherNode<T> newNode = new AnotherNode<T>(data);

            // 앞으로 넣을때 찾은 값이 머리였다면 머리에 넣기
            if (temp == Head)
            {
                Head.Prev = newNode;
                newNode.Next = Head;
                Head = newNode;
            }
            else
            {
                newNode.Prev = temp.Prev;
                newNode.Next = temp;
                temp.Prev.Next = newNode;
                temp.Prev = newNode;
            }
            
            Count++;
            return true;            
        }

        public bool Remove(AnotherNode<T> value)
        {
            // 찾는값이 하나인데 지워질때
            if (Head == Tail && value == Head)
            {
                Head = null;
                Tail = null;
                Count--;
                return true;
            }
            
            // 헤드가 지워질 때
            if (value == Head)
            {
                Head = Head.Next;
                Head.Prev = null;
                Count--;
                return true;
            }

            if (value == Tail)
            {
                Tail = Tail.Prev;
                Tail.Next = null;
                Count--;
                return true;
            }

            AnotherNode<T> temp = Head;
            while (temp != null)
            {
                if (temp == value)
                {
                    temp.Prev.Next = temp.Next;
                    temp.Next.Prev = temp.Prev;
                    Count--;
                    return true;
                }                
                temp = temp.Next;
            }

            // 끝까지 아무것도 안바꼈다면? false
            return false;
        }
        
        // Value를 넣으면 해당하는 노드 찾아 반환해주기
        public AnotherNode<T> FindAnotherNode(T value)
        {
            // 헤드나 테일이 없으면 뭔가 잘못되었다
            if (Head == null || Tail == null)
            {
                return null;
            }

            AnotherNode<T> temp = Head;
            while (temp != null)
            {
                if (temp.Data.Equals(value))
                {
                    return temp;
                }
                temp = temp.Next;
            }
            // 끝까지 찾아도 없으면 패스
            Console.WriteLine("못찾겠습니다 쥐쥐~~~");
            return null;
        }

        public void Clear()
        {
            _count = 0;
            _head = null;
            _tail = null;
        }
    }
```

## Main.cs
```cs
    AnotherLinkedList<string> al = new AnotherLinkedList<string>();            
    
    for (int i = 0; i < 5; i++)
    {
        AnotherNode<string> an = new AnotherNode<string>(i + "번");
        al.AddFirst(an);
    }
    PrintLinkedList(al);
    
    for (int i = 0; i < 5; i++)
    {
        AnotherNode<string> an = new AnotherNode<string>(i + "뒤번");
        al.AddLast(an);
    }
    PrintLinkedList(al);
    
    Console.WriteLine(al.FindAnotherNode("2번").Data);
    // 없는거 찾기
    Console.WriteLine(al.FindAnotherNode("6번")?.Data);
    Console.WriteLine("=====");
    // 일단 클리어
    al.Clear();
    al.AddFirst(new AnotherNode<string>("1번"));
    al.AddFirst(new AnotherNode<string>("2번"));
    al.AddAfter(al.FindAnotherNode("1번"), "3번");
    al.AddBefore(al.FindAnotherNode("2번"), "10번");
    al.AddBefore(al.FindAnotherNode("3번"), "9번");
    
    PrintLinkedList(al);
    PrintLinkedListBackward(al);

    bool remove1 = al.Remove(al.FindAnotherNode("3번"));
    bool remove2 = al.Remove(al.FindAnotherNode("10번"));
    bool remove3 = al.Remove(al.FindAnotherNode("1번"));
    bool remove4 = al.Remove(al.FindAnotherNode("9번"));
    bool remove5 = al.Remove(al.FindAnotherNode("2번"));
    PrintLinkedList(al);
    PrintLinkedListBackward(al);
    Console.WriteLine($"remove result {remove1},{remove2},{remove3},{remove4},{remove5}");
    
    al.AddFirst(new AnotherNode<string>("999번"));
    // 아마 안지워지겠지?
    al.Remove(new AnotherNode<string>("999번"));
    Console.WriteLine("안지워져");
    Console.WriteLine(al.Head.Data);

    // 지워지겠지?
    al.Remove(al.FindAnotherNode("999번"));
    Console.WriteLine("지워져");
    Console.WriteLine(al.Head?.Data);

    public static void PrintLinkedList(AnotherLinkedList<string> al)
    {
        AnotherNode<string> head = al.Head;
        while (head != null)
        {
            Console.WriteLine(head.Data);
            head = head.Next;
        }
        Console.WriteLine("======");
    }

    public static void PrintLinkedListBackward(AnotherLinkedList<string> al)
    {
        AnotherNode<string> tail = al.Tail;
        while (tail != null)
        {
            Console.WriteLine(tail.Data);
            tail = tail.Prev;
        }
        Console.WriteLine("======");
    }
```