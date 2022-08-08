# Segment Tree

세그먼트 트리는 말그대로 구간(segment)에 대한 것들을 처리할 때 사용할 수 있는 특수한 트리이다.

이진트리의 형태를 하고 있으며 구간합을 구하는 가장 빠른 구조이다.(시간복잡도: O(logN))



그리고 세그먼트 트리의 리프 노드와 리프 노드가 아닌 다른 노드는 다음과 같은 의미를 가진다.

- 리프 노드: 배열의 수 자체
- 다른 노드: 왼쪽 자식과 오른쪽 자식의 합



또한 어떤 노드의 번호가 x라고 할때 자식 노드의 번호는 다음과 같다.

- 왼쪽 자식 노드 번호: 2 * x
- 오른쪽 자식 노드 번호: 2 * x + 1



![seg](https://user-images.githubusercontent.com/55227276/170446128-759dad83-ce81-4392-ac5d-56971ff8a80d.png)

> 왼쪽은 배열의 합
>
> 오른쪽은 노드 번호

## Reference

- https://www.acmicpc.net/blog/view/9
- https://velog.io/@kimdukbae/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%8A%B8-%ED%8A%B8%EB%A6%AC-Segment-Tree
