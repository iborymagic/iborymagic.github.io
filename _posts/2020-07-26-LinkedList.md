---
layout: post
title: 자료구조 - Linked List
tags:
  - 자료구조
  - TIL
  - Linked List
---
리스트 자료구조는 데이터를 순차적으로 저장함.
데이터의 중복을 허용함.
크게 연결 리스트(Linked List)와 배열 리스트(Array List)로 나뉨.

## 배열 리스트(Array List)
대부분의 프로그래밍 언어에서 기본으로 지원하는 간단한 자료구조.  
동일한 데이터 타입을 연속적으로 저장한 자료구조.  
자바스크립트의 배열은 여러 가지 타입의 데이터들을 한 배열에 넣을 수 있음.  

사용하기 쉽고 데이터 참조가 빠르다는 장점.  

하지만 자바스크립트를 제외한 대부분의 언어에서는  
배열의 크기가 고정되어야 함.  
그래서 메모리 낭비가 심할 가능성이 높음.  
또, 배열의 처음이나 중간에서 원소를 넣고 빼는 연산이 비쌈.  

## 연결 리스트(Linked List)
배열과 유사하게 데이터를 순차적으로 저장함.  
하지만 원소들이 메모리 상에 연속적으로 위치하지 않음.  

노드(Node) 기반의 자료구조.  
노드는 자기 자신의 데이터와 다음 노드를 가리키는 포인터를 담고 있다.  
그래서, '데이터를 저장할 장소' + '다른 변수를 가리키기 위한 장소'에 각각  
별도의 메모리가 필요하다.  

* 연속되는 항목들이 포인터로 연결
* 마지막 항목은 null을 가리킴.
* 런타임에 크기가 커지거나 작아질 수 있음(가변 길이)
* 메모리가 허용하는 한 계속해서 길어질 수 있음
* 포인터를 위한 추가 메모리 공간이 필요
* 그 밖에는 메모리 공간을 낭비하지 않음

리스트의 처음이나 중간에 데이터를 넣고자 할 때에도  
기존의 연결을 끊고 새로운 연결을 해주기만 하면 되기 때문에 삽입, 삭제가 용이.  
반면, 노드를 차례대로 타고 가면서 검색을 해야 하기 때문에 탐색 속도가 떨어진다.  
(5번 노드의 데이터를 찾기 위해서는 1, 2, 3, 4, 5번 노드를 모두 거쳐야 한다)  

탐색 또는 정렬을 자주 한다면 배열을  
삽입 삭제가 잦은 데이터라면 연결리스트를 사용하는 것이 유리.  

### Head Pointer
모든 연결 리스트는 head를 가진다.  
Head pointer는 head node(첫 번째 노드)를 가리킨다.  
그 말은, head는 연결 리스트의 크기에 포함되지 않는다는 뜻.  
마지막 노드는 null을 가리킨다(그 다음 노드가 없다는 뜻).

처음 노드를 만들면 head pointer는 null을 가리키게 되는데,  
이 때도 역시나 마지막 pointer는 null을 가리킨다는 것을 알 수 있음.  
(Head pointer = last pointer니깐)  

head가 첫 번째 노드를 가리키는 순간, head값은  
첫 번째 노드를 return하게 된다.  

head === null이라는 건, 아무런 노드도 삽입되지 않은 상태임을 의미.  
(length = 0)  

### currentNode
연결 리스트에서는 항상 처음부터 링크를 타고 순차적으로 탐색을 해야 하기 때문에  
currentNode 변수를 따로 만들어서, head부터 시작해서 쭉 탐색을 해나간다.  
currentNode가 마지막 노드에 도달하게되면,  
currentNode.next가 null을 가리키게 된다.  
그래서 while문의 조건으로 currentNode.next를 사용함.  

삽입을 구현할 때는, 항상 리스트의 마지막에 삽입을 해줘야 하므로  
currentNode.next가 null을 가리키는 순간, currentNode.next에다가  
새로운 노드를 삽입해준다.  

### 연결리스트의 구현
addAt 메소드는 맨 끝에 추가하는 건 불가능하다.  
맨 끝에 추가하는 메소드는 add 메소드가 이미 존재한다.  

removeAt에 보면 index >= length이면 return null인데,  
등호가 들어가는 이유는, 배열과 마찬가지로 첫 번째 노드가  
0번 index이기 때문. 마지막 노드는 length - 1번 index.  

```javascript
function LinkedList() {
    let length = 0;
    let head = null;

    const Node = function(element) {
        this.element = element;
        this.next = null;
    };

    this.size = function() {
        return length;
    }

    this.head = function() {
        return head;
    }

    this.add = function(element) {
        const node = new Node(element);
        if(head === null) {
            head = node;
        } else {
            let currentNode = head;

            while(currentNode.next) {
                currentNode = currentNode.next;
            }

            currentNode.next = node;
        }
        length++;
    };

    this.remove = function(element) {
        let currentNode = head;
        let previousNode;
        if(currentNode.element === element) {
            head = currentNode.next;
        } else {
            while(currentNode.element !== element) {
                previousNode = currentNode;
                currentNode = currentNode.next;
            }

            previousNode.next = currentNode.next;
        }
        length--;
    };

    this.isEmpty = function() {
        return length === 0;
    };

    this.indexOf = function(element) {
        let currentNode = head;
        let index = -1;

        while(currentNode) {
            index++;
            if(currentNode.element === element) {
                return index;
            }
            currentNode = currentNode.next;
        }
        return -1;
    };

    this.elementAt = function(index) {
        let currentNode = head;
        let count = 0;
        while(count < index) {
            count++;
            currentNode = currentNode.next;
        }
        return currentNode.element;
    };

    this.addAt = function(index, element) {
        const node = new Node(element);

        let currentNode = head;
        let previousNode;
        let currentIndex = 0;

        if(index > length) {
            return false;
        }

        if(index === 0) {
            node.next = currentNode;
            head = node;
        } else {
            while(currentIndex < index) {
                currentIndex++;
                previousNode = currentNode;
                currentNode = currentNode.next;
            }
            node.next = currentNode;
            previousNode.next = node;
        }
        length++;
    }

    this.removeAt = function(index) {
        let currentNode = head;
        let previousNode;
        let currentIndex = 0;
        if(index < 0 || index >= length) {
            return null;
        }

        if(index === 0) {
            head = currentNode.next;
        } else {
            while(currentIndex < index) {
                currentIndex++;
                previousNode = currentNode;
                currentNode = currentNode.next;
            }
            previousNode.next = currentNode.next;
        }
        length--;
        return currentNode.element;
    };
}

const conga = new LinkedList();
conga.add('Kitten');
conga.add('Puppy');
conga.add('Dog');
conga.add('Cat');
conga.add('Fish');
console.log(conga.size());
console.log(conga.removeAt(3));
console.log(conga.elementAt(3));
console.log(conga.indexOf('Puppy'));
console.log(conga.size());
```

출처 1 : [소년코딩 블로그](https://boycoding.tistory.com/33)  
출처 2 : [freeCodeCamp 유튜브](https://www.youtube.com/watch?v=9YddVVsdG5A)  
