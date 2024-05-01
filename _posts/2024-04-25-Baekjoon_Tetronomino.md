---
title: "Tetromino 14500번"
date: 2024-04-25 02:23:00 +0900
categories: [Algorithm]
tags: [CodingTest, Algorithm, Graph, Tetris]
---
# 1. 문제

폴리오미노란 크기가 1×1인 정사각형을 여러 개 이어서 붙인 도형이며, 다음과 같은 조건을 만족해야 한다.

- 정사각형은 서로 겹치면 안 된다.
- 도형은 모두 연결되어 있어야 한다.
- 정사각형의 변끼리 연결되어 있어야 한다. 즉, 꼭짓점과 꼭짓점만 맞닿아 있으면 안 된다.

정사각형 4개를 이어 붙인 폴리오미노는 테트로미노라고 하며, 다음과 같은 5가지가 있다.

<center>
<img src="https://upload.wikimedia.org/wikipedia/commons/5/50/All_5_free_tetrominoes.svg" width="720" height=""/>
<p><b>[그림1]. 5가지 테트로미노 </b></p>
</center>

아름이는 크기가 N×M인 종이 위에 테트로미노 하나를 놓으려고 한다. 종이는 1×1 크기의 칸으로 나누어져 있으며, 각각의 칸에는 정수가 하나 쓰여 있다.

테트로미노 하나를 적절히 놓아서 테트로미노가 놓인 칸에 쓰여 있는 수들의 합을 최대로 하는 프로그램을 작성하시오.

테트로미노는 반드시 한 정사각형이 정확히 하나의 칸을 포함하도록 놓아야 하며, 회전이나 대칭을 시켜도 된다.

## 1.1. 입력

첫째 줄에 종이의 세로 크기 N과 가로 크기 M이 주어진다. (4 ≤ N, M ≤ 500)

둘째 줄부터 N개의 줄에 종이에 쓰여 있는 수가 주어진다. i번째 줄의 j번째 수는 위에서부터 i번째 칸, 왼쪽에서부터 j번째 칸에 쓰여 있는 수이다. 입력으로 주어지는 수는 1,000을 넘지 않는 자연수이다.

## 1.2. 출력

첫째 줄에 테트로미노가 놓인 칸에 쓰인 수들의 합의 최댓값을 출력한다.

# 2. 문제 해석

오브젝트 : Tetromino 5가지, Board(NxM)

조건
- 정사각형은 서로 겹치면 안된다.
- 도형은 모두 연결되어 있어야 한다.
- 정사각형의 변끼리 연결되어 있어야 한다.
- 폴리오미노는 총 5가지 종류
- 폴리오미노의 배치는 Rotate와 Decalcomanie가 가능하다.
- 종이(Board)는 N x M 크기를 가진다.
- 종이의 한칸 크기는 1x1 즉, 폴리오미노의 단위 크기이다.
- 종이의 한칸에는 정수가 하나 쓰여있다.
- 정수는 1~1000의 범위를 가진다.
- 2번째 줄의 5번째 수는 위에서부터 2번째 칸, 왼쪽에서부터 5번째 칸의 수이다. 
- (4 ≤ N, M ≤ 500)

# 3. 문제 해결 방안
## 3.1. Board 추상화
테트로미노는 모두 연결되어 있는 도형이다. 즉 배열을 Graph로 만들고 RL탐색을 통해 구현해볼 수 있을 것이다.

<center>
<img src="https://github.com/cisco/openh264/assets/56510688/59009ea2-86c4-436e-8ade-c08291717951" width="720" height=""/>
<p><b>[그림2]. Board 추상화 </b></p>
</center>

## 3.2. 테트로미노 구현

테트로미노는 블럭마다 정해진 순서의 Node 탐사를 진행한다. 원래는 Down Left, Down Right, Up Left, Up Right다 구현하려고 했는데 아래 아이디어 떠올라서 Down Left를 L, Down Right를 R로 함수를 생성해서 진행할 것이다.

<center>
<img src="https://github.com/cisco/openh264/assets/56510688/47e8a94f-4ad0-421a-973c-b3f8bfdef1a4" width="720" height=""/>
<p><b>[그림3]. 테트로미노 </b></p>
</center>

## 3.3. 테트로미노 Rotate와 Decalcomanie 구현

조건 중에서 폴리오미노는 Decalcomanie와 Rotate가 가능한데, 나는 귀찮으니까 Board를 90도씩 회전하는 아이디어를 사용할 것이다. 그러면 시계 방향으로 90도 회전한 Board에서는 Block도 90도 Rotate한 효과를 발휘할 것이고 180도 회전한 Board에서는 Decalcomanie가 된 효과를 발휘할 것이다.

근데 270도 회전한것도 해줘야하는지 잘 모르겠어서 일단 180도까지만 진행해보았습니다.

* 추가 아이디어로 보드 회전 시 정 중앙값은 고정되므로 해당 값을 기준으로 회전하려 했는데 이거는 정방행렬일때만 가능한 아이디어이므로 기각했습니다.

<center>
<img src="https://github.com/cisco/openh264/assets/56510688/c588e71d-42ee-443d-9e47-175bc57dd3b0" width="720" height=""/>
<p><b>[그림4]. Rotating Board </b></p>
</center>

## 3.4. 시간복잡도를 위한 최적화

만약 보드를 회전시키면서 한배열씩 블럭을 두고 해당 값이 최대값인지 확인하는 알고리즘을 사용할 시 시간복잡도는 최악일 경우O(N^2)이 될 가능성이 크다.
그래서 나는 단위 칸당 가지고 있는 정수의 최대 최소 평균값 이상의 칸에 대해서만 블럭을 놓는 최적화를 진행할 것이다.

- ex. 보드에 정수가 1~5의 범위를 가지고 있을 시 3~5에 해당하는 칸에만 블럭 배치 시도

#### 1번케이스

```
시간복잡도 = N*M*5*5
N*M은 보드의 크기
5는 Block Type이 5개므로 상수
다른 5는 Board, CW90Board, RCW90Board, CW180Board, RCW180Board 총 5가지 Board Type이 있으므로 상수
```

#### 2번케이스

```
시간복잡도 = C*5*5
C는 평균값 이상의 칸의 갯수
5는 Block Type이 5개므로 상수
다른 5는 Board, CW90Board, RCW90Board, CW180Board, RCW180Board 총 5가지 Board Type이 있으므로 상수
```

# 4. 소스코드

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#define TETRO_SIZE 4
#define ROWS 5
#define COLS 5
//(4 ≤ N, M ≤ 500)
#define N 500
#define M 500

typedef struct {
    int value;
    int isUpperThanAver;
}TILE;

typedef struct{
	TILE tile[N][M];
}BOARD;

typedef struct {
	int array[TETRO_SIZE]; //배열의 Value 중 0은 Right, 1은 Left뜻함.
}TETRO;

typedef struct Node {
    int data;
    struct Node* right;
    struct Node* left;
} Node;

// 그래프의 노드 생성 함수
Node* createNode(int data) {
    Node* newNode = (Node*)malloc(sizeof(Node));
    newNode->data = data;
    newNode->right = NULL;
    newNode->left = NULL;
    return newNode;
}

// 배열을 그래프로 변환하는 함수
Node* arrayToGraph(int arr[][M]) {
    Node* graph[ROWS][M];

    // 각 배열 요소를 그래프 노드로 변환
    for (int i = 0; i < ROWS; i++) {
        for (int j = 0; j < M; j++) {
            graph[i][j] = createNode(arr[i][j]);
        }
    }

    // 이웃한 노드들 간의 연결 설정
    for (int i = 0; i < ROWS; i++) {
        for (int j = 0; j < M; j++) {
            if (i < ROWS - 1)
                graph[i][j]->left = graph[i + 1][j];
            if (j < M - 1)
                graph[i][j]->right = graph[i][j + 1];
        }
    }

    // 그래프의 시작 노드 반환
    return graph[0][0];
}

// 그래프를 90도 회전하는 함수
Node* rotateGraph(Node* graph) {
    Node* newGraph[M][N];

    // 새로운 그래프 생성
    for (int i = 0; i < M; i++) {
        for (int j = 0; j < N; j++) {
            newGraph[i][j] = createNode(0); // 초기화
        }
    }

    // 그래프의 노드를 회전하여 새로운 그래프에 설정
    Node* currentNode = graph;
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < M; j++) {
            newGraph[j][N - 1 - i]->data = currentNode->data;
            currentNode = currentNode->right;
        }
        currentNode = currentNode->left;
    }

    // 이웃한 노드들 간의 연결 설정
    for (int i = 0; i < M; i++) {
        for (int j = 0; j < N; j++) {
            if (i < M - 1)
                newGraph[i][j]->right = newGraph[i + 1][j];
            if (j < N - 1)
                newGraph[i][j]->left = newGraph[i][j + 1];
        }
    }

    // 새로운 그래프의 시작 노드 반환
    return newGraph[0][0];
}

// 그래프를 배열로 변환하여 출력하는 함수
void printGraph(Node* graph) {
    Node* currentRow = graph;

    while (currentRow != NULL) {
        Node* currentNode = currentRow;
        while (currentNode != NULL) {
            printf("%d ", currentNode->data);
            currentNode = currentNode->right;
        }
        printf("\n");
        currentRow = currentRow->left;
    }
}
void initBoard(BOARD* b)
{
    for (int i = 0; i < N; i++)
    {
        for (int j = 0; j < M; j++)
        {
            b->tile[N][M].isUpperThanAver = 0;
            b->tile[N][M].value = 0;
        }
    }
}

int main()
{
    //init phase
	BOARD* B;
	B = (BOARD*)malloc(sizeof(BOARD));
    void initBoard(B);
	int n=0, m=0;
    int minValue, maxValue = 0; // tile의 최소, 최대값
    int max = 0; //블럭이 놓인 tile value의 최대값
    int cmax = 0; //블럭이 놓인 tile value의 현재 최대값
    //input phase
	scanf("%d %d", &n, &m);
	for (int i = 0; i < n; i++)
	{
		for (int j = 0; i < m; j++)
		{
			scanf("%d ", &B->tile[j][i].value);
            // 최소값과 최대값 갱신
            if (i == 0 && j == 0) // 첫 번째 값으로 초기화
            { 
                minValue = maxValue = B->tile[j][i].value;
            }
            else 
            {
                if (B->tile[j][i].value < minValue)
                    minValue = B->tile[j][i].value;
                if (B->tile[j][i].value > maxValue)
                    maxValue = B->tile[j][i].value;
            }
		}
	}
    int averageValue = (maxValue + minValue) / 2;
    //평균값보다 높은 정수 타일에 표시
    for (int i = 0; i < n; i++)
    {
        for (int j = 0; i < m; j++)
        {
            if (B->tile[j][i].value > averageValue)
                B->tile[j][i].isUpperThanAver = 1;
        }
    }
    //Compare phase
    while (1)
    {
        max = searchBoard(B, T);
        if (max > cmax)
            cmax = max;
    }
    //serach phase
    



	free(B);
	return 0;
}
```

코드 작성 중..