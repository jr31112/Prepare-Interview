# 이진 트리(Binary Tree)

## 1. 이진 트리

두 개의 서브 트리(큰 트리에 속하는 작은 트리)로 나뉜 트리 나뉘어진 두 서브 트리도 이진트리여야함

 ![tree](./tree01) 

1. 전 이진 트리(Full Binary Tree)

   ​	![full binary tree](./tree02) 

   * 모든 노드가 0개 또는 2개의 자식 노드를 갖는 트리.

2. **완전 이진 트리(Complete Binary Tree)**

    ![complete](./tree03)

   * 트리의 모든 높이에서 노드가 꽉 차 있는 이진 트리. 즉, 마지막 레벨을 제외하고 모든 레벨이 완전히 채워져 있다.
   * 마지막 레벨은 꽉 차 있지 않아도 되자만 노드가 왼쪽에서 오른쪽으로 채워져야 한다.
   * 마지막 레벨 h에서 (1 ~ 2h-1)개의 노드를 가질 수 있다.
   * 또 다른 정의는 가장 오른쪽의 잎 노드가 (아마도 모두) 제거된 포화 이진 트리다.
   * 완전 이진 트리는 배열을 사용해 효율적으로 표현 가능하다.
   
3. 포화 이진 트리(Perfect Binary Tree)

   ![perfect binary tree](C:\Users\jr435\Desktop\면접\datastructure\tree04)

   * 전 이진 트리이면서 완전 이진 트리인 경우
   * 모든 말단 노드는 같은 높이에 있어야 하며, 마지막 단계에서 노드의 개수가 최대가 되어야 한다.
   * 모든 내부 노드가 두 개의 자식 노드를 가진다.
   * 모든 말단 노드가 동일한 깊이 또는 레벨을 갖는다.
   * 노드의 개수가 정확히 2^(k-1)개여야 한다. (k: 트리의 높이) -> 흔하게 나타나는 경우가 아니다. 즉, 이진 트리가 모두 포화 이진 트리일 것이라고 생각하지 않는다.

4. 이진 탐색 트리(Binary Search Tree, BST)

   ![binary search tree](./tree05)

   * 이진탐색트리란 이진탐색(binary search)과 연결리스트(linked list)를 결합한 자료구조의 일종

   * 규칙 1. 이진 탐색 트리의 노드에 저장된 키는 유일하다.

   * 규칙 2. 부모의 키가 왼쪽 자식 노드의 키보다 크다.

   * 규칙 3. 부모의 키가 오른쪽 자식 노드의 키보다 작다.

   * 규칙 4. 왼쪽과 오른쪽 서브트리도 이진 탐색 트리이다.

   * 편향 Tree가 될 수 있으며 이때 시간 복잡도는 O(n)이 된다.

     -> 결국 리스트와 다를 바가 없다.

5. 트라이(Trie)

   ![trie](./tree06) 

   * n-차 트리(n-ary Tree)의 변종
   * 각 노드에 문자를 저장하는 자료구조
   * 따라서 트리를 아래쪽으로 순회하면 단어 하나가 나온다.
   * 접두사를 빠르게 찾아보기 위한 흔한 방식 으로, 모든 언어를 트라이에 저장해 놓는 방식이 있다.
   * 유효한 단어 집합을 이용 하는 많은 문제들은 트라이를 통해 최적화할 수 있다.

6. 이진 힙(Binary Heap)

    * 최대 최소값을 쉽게 찾아내기 위한 자료구조
    * 완전 이진트리 기반으로 구성
    * 루트에 최소(최대)값이 들어간다!

    삽입(O(log(n)))

    ![heap push](./tree07)

    삭제(O(log(n)))

    ![heap pop](./tree08)

7. 레드블랙 트리(Red-Black Tree)

    ![red-black tree](./tree09) 

    RBT(Red-Black Tree)는 BST 를 기반으로하는 트리 형식의 자료구조이다. 결론부터 말하자면 Red-Black Tree 에 데이터를 저장하게되면 Search, Insert, Delete 에 O(log n)의 시간 복잡도가 소요된다. 동일한 노드의 개수일 때, depth 를 최소화하여 시간 복잡도를 줄이는 것이 핵심 아이디어이다. 동일한 노드의 개수일 때, depth 가 최소가 되는 경우는 tree 가 complete binary tree 인 경우이다.

    * Red-Black Tree 의 정의

      Red-Black Tree 는 다음의 성질들을 만족하는 BST 이다.

      1. 각 노드는 `Red` or `Black`이라는 색깔을 갖는다.
      2. Root node 의 색깔은 `Black`이다.
      3. 각 leaf node 는 `black`이다.
      4. 어떤 노드의 색깔이 `red`라면 두 개의 children 의 색깔은 모두 black 이다.
      5. 모든 리프노드에서 **Black-Height는 같다.** 
     * Black-Height: 노드 x 로부터 노드 x 를 포함하지 않은 leaf node 까지의 simple path 상에 있는 black nodes 들의 개수
    
* Red-Black Tree 의 특징
  
      1. Binary Search Tree 이므로 BST 의 특징을 모두 갖는다.
      2. Root node 부터 leaf node 까지의 모든 경로 중 최소 경로와 최대 경로의 크기 비율은 2 보다 크지 않다. 이러한 상태를 `balanced` 상태라고 한다.
  3. 노드의 child 가 없을 경우 child 를 가리키는 포인터는 NIL 값을 저장한다. 이러한 NIL 들을 leaf node 로 간주한다.
  
  *RBT 는 BST 의 삽입, 삭제 연산 과정에서 발생할 수 있는 문제점을 해결하기 위해 만들어진 자료구조이다. 이를 어떻게 해결한 것인가?*
  
* 삽입
  
  우선 BST 의 특성을 유지하면서 노드를 삽입을 한다. 그리고 삽입된 노드의 색깔을 **RED 로** 지정한다. Red 로 지정하는 이유는 Black-Height 변경을 최소화하기 위함이다. 삽입 결과 RBT 의 특성 위배(violation)시 노드의 색깔을 조정하고, Black-Height 가 위배되었다면 rotation 을 통해 height 를 조정한다. 이러한 과정을 통해 RBT 의 동일한 height 에 존재하는 internal node 들의 Black-height 가 같아지게 되고 최소 경로와 최대 경로의 크기 비율이 2 미만으로 유지된다.
  
* 삭제
  
  삭제도 삽입과 마찬가지로 BST 의 특성을 유지하면서 해당 노드를 삭제한다. 삭제될 노드의 child 의 개수에 따라 rotation 방법이 달라지게 된다. 그리고 만약 지워진 노드의 색깔이 Black 이라면 Black-Height 가 1 감소한 경로에 black node 가 1 개 추가되도록 rotation 하고 노드의 색깔을 조정한다. 지워진 노드의 색깔이 red 라면 Violation 이 발생하지 않으므로 RBT 가 그대로 유지된다.