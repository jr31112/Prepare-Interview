# Linked List & Tree 기본

## 1. Linked List

1. List(연결 리스트)란?

   > 순서를 가진 데이터의 집합, 동일한 데이터를 가지고 있어도 상관 없음
   >
   > 같은 크기의 동일한 자료형(c 기준)

   1. 순차 List의 문제점

      - 자료의 삽입/삭제 과정에서 원소들을 이동시키는 작업이 필요해 시간이 크게 필요하다.
      - 배열의 크기가 정해져 있을 경우 너무 크게 할당하면 메모리의 낭비가 발생하며 반대로 너무 적게 할당 하면 overflow가 발생하거나 새롭게 배열을 만들어 작업해야함.

   2. 연결 List(Linked List)란?

      - 순차 List와는 다르게 메모리 상 물리적인 순서가 일치 하지 않는다.
      - 링크를 통해 원소에 접근하므로, 순차 List처럼 원소를 이동시키는 작업이 필요하지 않는다.
      - 배열의 크기를 정하지 않고 자료구조의 크기를 동적으로 조정하여 메모리의 효율적인 사용이 가능하다.

   3. 연결 List의 구조

      ![linked list](./list01)

      1. 구성 요소
         - 노드(Node) : 연결 List에서 하나의 원 소에 필요한 데이터를 갖고 있는 자료 단위
           - 데이터 필드 : 원소의 값을 저장하는 곳
           - 링크 필드 : 다음 노드의 주로를 저장하는 자료구조
         - 헤드(Head) : List의 처음 노드를 나타내는 레퍼런스
      2. 연결 구조
         - 노드가 링크 필드에 의해 다음 노드로 연결도는 구조를 가진다.
         - **헤드가 가장 앞 노드를 가리키고 링크 필드가 다음 노드를 가리킨다.**
           - **최종적으로 링크 필드가 Null인 노드가 리스트의 가장 마지막 노드가 된다.**
      3. 삽입 연산
         1. 메모리를 할당하여 새로운 노드 생성
         2. 삽입될 위치 바로 앞에 위치한 노드의 링크 필드 복사
         3. 새로운 노드의 주소를 앞 노드의 링크 필드에 저장
      4. 삭제 연산
         1. 삭제할 노드의 앞 노드 탐색
         2. 삭제할 노드의 링크 필드를 선행 노드의 링크 필드에 복사

## 2. Tree

1. Tree란?

   * 트리는 싸이클이 없는 무향 연결 그래프이다.
   * 스택이나 큐와 같은 선형 구조가 아닌 비선형 자료구조
   * 두 노드 사이에는 유일한 경로가 존재한다
   * 각 노드는 최대 하나의 부모 노드가 존재할 수 있다.
   * 각 노드는 자식 노드가 없거나 하나 이상이 존재 할 수 있다.

2. 트리 용어

   ![tree](./list02)

   * 노드(Node) : 트리를 구성하는 원소(자료)
   * 간선(Edge) : 노드를 연결하는 선
   * 차수(degree) : 한 노드가 가지는 서브 트리의 수, 즉 자식노드의 수
   * 루트 노드(Root Node) : 트리 구조에서 최상위에 있는 노드, 시작 노드
   * 단말 노드(Leaf Node, Terminal Node) : 하위에 다른 노드가 연결되어 있지 않은 노드
   * 조상 노드 : 자기와 연결된 간선을 따라 위로 올라가면서 만나는 노드
   * 자식 노드 : 자기와 연결된 간선을 따라 아래로 내려가면서 만나는 노드
   * 높이(Height) : 루트에서 그 노드에 이르는 경로에 있는 간선의 수

## 3. 순회 방식

![traverse](/list03)

* 전위 순회(preorder traverse) : 뿌리(root)를 먼저 방문

  0->1->3->7->8->4->9->10->2->5->11->6

* 중위 순회(inorder traverse) : 왼쪽 하위 트리를 방문 후 뿌리(root)를 방문

  7->3->8->1->9->4->10->0->11->5->2->6

* 후위 순회(postorder traverse) : 하위 트리 모두 방문 후 뿌리(root)를 방문

  7->8->3->9->10->4->1->11->5->6->2->0

* 층별 순회(level order traverse) : 위 쪽 node들 부터 아래방향으로 차례로 방문

  0->1->2->3->4->5->6->7->8->9->10->11

