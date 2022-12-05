---
title: 연결리스트(LinkedList)
categories:
  - DataStructure
tags:
  - Algorithm
  - 자료구조
  - LinkedList
  - DataStructure
toc: true
---

# 연결리스트(LinkedList)

## LinkedList란?
원소를 저장할 때 그다음 원소가 있는 위치를 포함하는 방식으로 저장하는 방식이다.  


## LinkedList의 성질
- K번째 원소를 확인/변경하기 위해 O(K)가 필요함
- 임의의 위치에 원소를 추가, 임의의 위치에 원소를 제거하기 위해 O(1) 또는 O(N)이 필요함  
- 원소들이 메모리상에 연속해있지 않아 캐시메모리적중률(Cache hit rate)이 낮지만, 할당이 다소 쉽다


## LinkedList의 종류
- 단일 연결 리스트(Singly Linked List)
    - 각 원소가 자신의 다음 원소의 주소를 들고 있는 LinkedList
- 이중 연결 리스트(Doubly Linked List)
    - 각 원소가 이전 원소와 다음 원소의 주소를 둘 다 들고 있는 LinkedList
- 원형 연결 리스트(Circular Linked List)
    - 끝과 처음이 연결된 LinkedList
    - 각 원소가 이중연결리스트처럼 이전과 다음 주소를 모두 들고 있어도 상관없다


## Array vs LinkedList  
  
|  | Array | LinkedList |  
| -- | ----- | ----------- |  
| K번째 원소 접근 | O(1) | O(K) |  
|임의 위치에 원소 추가/제거|O(N)|첫 번째와 마지막의 참조를 저장하고 있을 경우<br>- 첫 번째에 추가 O(1)<br>- 마지막에 추가 O(1)<br>- 임의의 위치까지 탐색 O(N), 추가 O(1) |  
|메모리상의 배치|	연속|	불연속|  
|추가적으로 필요한 공간(Overhead)|-|	O(N)<br>다음원소등의 주소값을 저장해야하므로, 32비트 컴퓨터라면 4N바이트가 필요|  


중간에 데이터를 추가나 삭제하더라도 전체의 인덱스가 한 칸씩 뒤로 밀리거나 당겨지는 일이 없기에  
 ArrayList에 비해서 데이터의 추가나 삭제가 용이하나 탐색속도가 떨어진다.  
탐색 또는 정렬을 자주 하는 경우엔 Array을 사용하고,  
데이터의 추가/ 삭제가 많은 경우 LinkedList를 사용하는것이 좋다.  


## LinkedList 구현하기
```java
class LinkedList<E> {
    private Node<E> first;
    private Node<E> last;
    private int size = 0;

    public LinkedList() {
    }

    private static class Node<E> {
        E item;
        Node<E> prev;
        Node<E> next;

        Node(Node<E> prev, E element, Node<E> next) {
            this.item = element;
            this.prev = prev;
            this.next = next;
        }
    }
```
첫 번째 node를 저장할 `first`, 마지막 node를 저장할 `last` 그리고 `size`를 저장할 변수를 선언한다.  
Node는 내부 클래스로 선언하며, item과, 이전 node, 다음 node를 갖는다.  

```java
private Node<E> node(int index) {
        if (index < (size >> 1)) {
            Node<E> x = first;
            for (int i = 0; i < index; i++)
                x = x.next;
            return x;
        } else {
            Node<E> x = last;
            for (int i = size - 1; i > index; i--)
                x = x.prev;
            return x;
        }
    }
```
node 함수는 index가 주어지면 해당 노드를 찾는 함수이다.  
size를 절반으로 나누어 절반보다 적다면 처음부터 탐색, 절반보다 크다면 마지막부터 탐색한다.  
예를 들어 size가 10이라면 1010(10) -> 101(5)이 되고,  
이때 주어진 index가 3이라면 0부터 탐색, 주어진 index가 7이라면 10부터 9, 8…. 순으로 탐색하게 된다.  

그다음 원소를 추가하는 함수를 구현하자.  
```java
    public boolean add(E element) {
        linkLast(element);
        return true;
    }

    public void add(int index, E element) {
        if (index == size) {
            linkLast(element);
        } else {
            linkBefore(element, node(index));
        }
    }

    public void addFirst(E element) {
        linkFirst(element);
    }

    public void addLast(E element) {
        linkLast(element);
    }
```
`add(element)` 함수는 마지막에 원소를 추가하며, boolean을 반환한다.   
`add(index, element)`는 index가 size와 같다면 마지막에 원소를 추가, 아니라면 주어진 index에 새 원소를 추가하고, 기존 원소는 새로 추가된 원소의 다음 순서가 된다.  
`addFirst(element)`는 첫 번째에 원소를 추가, `addLast(element)`는 마지막에 원소를 추가한다.  
`linkFirst`, `linkLast`, `linkBefore`의 구현은 아래와 같다.  
```java
 private void linkFirst(E e) {
        final Node<E> f = first;
        final Node<E> newNode = new Node<>(null, e, f);
        first = newNode;
        if (f == null) {
            last = newNode;
        } else {
            f.prev = newNode;
        }
        size++;
    }
```
linkFirst는 이전에 first로 저장하고 있던 Node앞에 새 노드를 추가한다.  
newNode는 이전에 저장하고 있던 first를 next로 가지게 된다.  

```java
private void linkLast(E e) {
        final Node<E> l = last;
        final Node<E> newNode = new Node<>(l, e, null);
        last = newNode;
        if (l == null) {
            first = newNode;
        } else {
            l.next = newNode;
        }
        size++;
    }
```
linkLast는 이전에 last로 저장하고 있던 Node뒤에 새 노드를 추가한다.  
newNode는 이전에 저장하고 있던 last를 prev로 가지게 된다.  

```java
    private void linkBefore(E e, Node<E> succ) {
        final Node<E> p = succ.prev;
        final Node<E> newNode = new Node<>(p, e, succ);
        succ.prev = newNode;
        if (p == null) {
            first = newNode;
        } else {
            p.next = newNode;
        }
        size++;
    }
```
linkBefore는 원소 `e`와 주어진 index로 찾은 노드(`node(index)`)를 인자로 받는다.  
기존 원소(succ)의 prev 새로운 원소 newNode의 prev가 되고, 
기존 원소(succ)는 새로운 원소 newNode의 next가 된다.  

다음은 제거 함수를 구현하자.  
```java
     public E remove(int index) {
        final Node<E> x = node(index);
        final E element = x.item;
        final Node<E> prev = x.prev;
        final Node<E> next = x.next;

        if (prev == null) {
            first = next;
        } else {
            prev.next = next;
            x.prev = null;
        }

        if (next == null) {
            last = prev;
        } else {
            next.prev = prev;
            x.next = null;
        }
        x.item = null;
        size--;
        return element;
    }
```
`remove(index)` 함수는 index의 노드를 제거한다.  
제거할 index 노드의 전, 후에 있는 노드들의 next와 prev를 재설정해준다.   

```java
    public E removeFirst() {
        final Node<E> x = first;
        final E element = x.item;
        final Node<E> next = x.next;
        x.item = null;
        x.next = null;
        first = next;
        if (next == null) {
            last = null;
        } else {
            next.prev = null;
        }

        size--;
        return element;
    }
```
`removeFirst(index)` 함수는 첫 번째 노드를 제거한다.  
그 다음 노드가 존재한다면, 다음 노드의 prev를 null로 비워준다.  

```java
    public E removeLast() {
        final Node<E> x = last;
        final E element = x.item;
        final Node<E> prev = x.prev;
        x.item = null;
        x.prev = null;
        last = prev;
        if (prev == null) {
            first = null;
        } else {
            prev.next = null;
        }
        size--;
        return element;
    }
```
`removeLast(index)` 함수는 마지막 노드를 제거한다.  
그 이전노드가 존재한다면, 다음 노드의 next를 null로 비워준다.  

```java
    public E set(int index, E element) {
        Node<E> x = node(index);
        E oldElement = x.item;
        x.item = element;
        return oldElement;
    }
```
set함수는 주어진 index의 node의 element를 업데이트한다.  

```java
    public E get(int index) {
        return node(index).item;
    }
```
get함수는 주어진 index의 node의 element를 반환한다.  

```java
    public void clear() {
        Node<E> x = first;
        while (x != null) {
            Node<E> next = x.next;
            x.item = null;
            x.next = null;
            x.prev = null;
            x = next;
        }
        first = last = null;
        size = 0;
    }
```
clear 함수는 첫 번째 노드부터 마지막 노드까지 순회하며,
모든 노드의 값들을 초기화한다.  

전체 코드  
```java
class LinkedList<E> {
    private Node<E> first;
    private Node<E> last;
    private int size = 0;

    public LinkedList() {
    }

    private static class Node<E> {
        E item;
        Node<E> prev;
        Node<E> next;

        Node(Node<E> prev, E element, Node<E> next) {
            this.item = element;
            this.prev = prev;
            this.next = next;
        }
    }

    public boolean add(E element) {
        linkLast(element);
        return true;
    }

    public void add(int index, E element) {
        if (index == size) {
            linkLast(element);
        } else {
            linkBefore(element, node(index));
        }
    }

    public void addFirst(E element) {
        linkFirst(element);
    }

    public void addLast(E element) {
        linkLast(element);
    }


    public E remove(int index) {
        final Node<E> x = node(index);
        final E element = x.item;
        final Node<E> prev = x.prev;
        final Node<E> next = x.next;

        if (prev == null) {
            first = next;
        } else {
            prev.next = next;
            x.prev = null;
        }

        if (next == null) {
            last = prev;
        } else {
            next.prev = prev;
            x.next = null;
        }
        x.item = null;
        size--;
        return element;
    }

    public E removeFirst() {
        final Node<E> x = first;
        final E element = x.item;
        final Node<E> next = x.next;
        x.item = null;
        x.next = null;
        first = next;
        if (next == null) {
            last = null;
        } else {
            next.prev = null;
        }

        size--;
        return element;
    }

    public E removeLast() {
        final Node<E> x = last;
        final E element = x.item;
        final Node<E> prev = x.prev;
        x.item = null;
        x.prev = null;
        last = prev;
        if (prev == null) {
            first = null;
        } else {
            prev.next = null;
        }
        size--;
        return element;
    }

    public E set(int index, E element) {
        Node<E> x = node(index);
        E oldElement = x.item;
        x.item = element;
        return oldElement;
    }

    public E get(int index) {
        return node(index).item;
    }

    public int size() {
        return size;
    }

    public void clear() {
        Node<E> x = first;
        while (x != null) {
            Node<E> next = x.next;
            x.item = null;
            x.next = null;
            x.prev = null;
            x = next;
        }
        first = last = null;
        size = 0;
    }

    private void linkFirst(E e) {
        final Node<E> f = first;
        final Node<E> newNode = new Node<>(null, e, f);
        first = newNode;
        if (f == null) {
            last = newNode;
        } else {
            f.prev = newNode;
        }
        size++;
    }

    private void linkLast(E e) {
        final Node<E> l = last;
        final Node<E> newNode = new Node<>(l, e, null);
        last = newNode;
        if (l == null) {
            first = newNode;
        } else {
            l.next = newNode;
        }
        size++;
    }

    private void linkBefore(E e, Node<E> succ) {
        final Node<E> p = succ.prev;
        final Node<E> newNode = new Node<>(p, e, succ);
        succ.prev = newNode;
        if (p == null) {
            first = newNode;
        } else {
            p.next = newNode;
        }
        size++;
    }

    private Node<E> node(int index) {
        if (index < (size >> 1)) {
            Node<E> x = first;
            for (int i = 0; i < index; i++)
                x = x.next;
            return x;
        } else {
            Node<E> x = last;
            for (int i = size - 1; i > index; i--)
                x = x.prev;
            return x;
        }
    }
}
```

## 예시
```java
class Main {
    public static void main(String[] args) {
        LinkedList<String> linkedList = new LinkedList<>();
        linkedList.add("20"); //20
        traverse(linkedList);
        linkedList.add(0, "10"); // 10 20
        traverse(linkedList);
        linkedList.addLast("25"); // 10 20 25
        traverse(linkedList);
        linkedList.addFirst("0"); // 0 10 20 25
        traverse(linkedList);
        linkedList.add("20");  // 0 10 20 25 30
        traverse(linkedList);

        linkedList.set(0, "5"); // 5 10 20 25 30
        traverse(linkedList);

        linkedList.remove(4); // 5 10 20 25
        traverse(linkedList);
        linkedList.removeFirst(); // 10 20 25
        traverse(linkedList);
        linkedList.removeLast(); // 10 20
        traverse(linkedList);

        linkedList.clear();
        traverse(linkedList);
    }

    static void traverse(LinkedList<String> linkedList) {
        for (int i = 0; i < linkedList.size(); i++) {
            System.out.print(linkedList.get(i) + " ");
        }
        System.out.println("\n");
    }
}
```