# ACM Craft

| 시간 제한 | 메모리 제한 | 제출  | 정답 | 맞은 사람 | 정답 비율 |
| :-------- | :---------- | :---- | :--- | :-------- | :-------- |
| 1 초      | 512 MB      | 30391 | 4587 | 2831      | 18.168%   |

## 문제

서기 2012년! 드디어 2년간 수많은 국민들을 기다리게 한 게임 ACM Craft (Association of Construction Manager Craft)가 발매되었다.

이 게임은 지금까지 나온 게임들과는 다르게 ACM크래프트는 다이나믹한 게임 진행을 위해 건물을 짓는 순서가 정해져 있지 않다. 즉, 첫 번째 게임과 두 번째 게임이 건물을 짓는 순서가 다를 수도 있다. 매 게임시작 시 건물을 짓는 순서가 주어진다. 또한 모든 건물은 각각 건설을 시작하여 완성이 될 때까지 Delay가 존재한다.

 

![img](https://www.acmicpc.net/upload/201003/star.JPG)

 

위의 예시를 보자.

이번 게임에서는 다음과 같이 건설 순서 규칙이 주어졌다. 1번 건물의 건설이 완료된다면 2번과 3번의 건설을 시작할수 있다. (동시에 진행이 가능하다) 그리고 4번 건물을 짓기 위해서는 2번과 3번 건물이 모두 건설 완료되어야지만 4번건물의 건설을 시작할수 있다.

따라서 4번건물의 건설을 완료하기 위해서는 우선 처음 1번 건물을 건설하는데 10초가 소요된다. 그리고 2번 건물과 3번 건물을 동시에 건설하기 시작하면 2번은 1초뒤에 건설이 완료되지만 아직 3번 건물이 완료되지 않았으므로 4번 건물을 건설할 수 없다. 3번 건물이 완성되고 나면 그때 4번 건물을 지을수 있으므로 4번 건물이 완성되기까지는 총 120초가 소요된다.

프로게이머 최백준은 애인과의 데이트 비용을 마련하기 위해 서강대학교배 ACM크래프트 대회에 참가했다! 최백준은 화려한 컨트롤 실력을 가지고 있기 때문에 모든 경기에서 특정 건물만 짓는다면 무조건 게임에서 이길 수 있다. 그러나 매 게임마다 특정건물을 짓기 위한 순서가 달라지므로 최백준은 좌절하고 있었다. 백준이를 위해 특정건물을 가장 빨리 지을 때까지 걸리는 최소시간을 알아내는 프로그램을 작성해주자.

 

## 입력

첫째 줄에는 테스트케이스의 개수 T가 주어진다. 각 테스트 케이스는 다음과 같이 주어진다. 첫째 줄에 건물의 개수 N 과 건물간의 건설순서규칙의 총 개수 K이 주어진다. (건물의 번호는 1번부터 N번까지 존재한다) 

둘째 줄에는 각 건물당 건설에 걸리는 시간 D가 공백을 사이로 주어진다. 셋째 줄부터 K+2줄까지 건설순서 X Y가 주어진다. (이는 건물 X를 지은 다음에 건물 Y를 짓는 것이 가능하다는 의미이다) 

마지막 줄에는 백준이가 승리하기 위해 건설해야 할 건물의 번호 W가 주어진다. (1 ≤ N ≤ 1000, 1 ≤ K ≤ 100000 , 1≤ X,Y,W ≤ N, 0 ≤ D ≤ 100000)

## 출력

건물 W를 건설완료 하는데 드는 최소 시간을 출력한다. 편의상 건물을 짓는 명령을 내리는 데는 시간이 소요되지 않는다고 가정한다.

모든 건물을 지을 수 없는 경우는 없다.



## 예제 입력 1 복사

```
2
4 4
10 1 100 10
1 2
1 3
2 4
3 4
4
8 8
10 20 1 5 8 7 1 43
1 2
1 3
2 4
2 5
3 6
5 7
6 7
7 8
7
```

## 예제 출력 1 복사

```
120
39
```



## 출처

<https://www.acmicpc.net/problem/1005>



## 풀이

Dp, DFS를 사용했다

### 입력  코드

```
std::vector<Building> buildings{std::move(analyseInputData())};
std::cin >> willBuildBuilding;
```

```c++
std::vector<Building> analyseInputData() {
    int buildingNum, buildedRuleNum;
    std::cin >> buildingNum >> buildedRuleNum;

    std::vector<Building> buildings(buildingNum, Building{0, std::vector<int>(), false});

    for (int i = 0; i < buildingNum; ++i) {
        std::cin >> buildings[i].time;
    }

    for (int i = 0; i < buildedRuleNum; ++i) {
        int building, preBuilded;
        std::cin >> preBuilded >> building;
        buildings[building - 1].preBuilded.push_back(preBuilded - 1);
    }

    return buildings;
}
```



### 출력 코드

```c++
int time = findMinTimeForBuild(buildings, willBuildBuilding - 1);

std::cout << time << '\n';
```



### DFS, DP

```c++
int findMinTimeForBuild(std::vector<Building> &buildings, int willBuildBuilding) { 
    if (buildings[willBuildBuilding].isCal)
        return buildings[willBuildBuilding].time;
    
    buildings[willBuildBuilding].isCal = true;

    if (buildings[willBuildBuilding].preBuilded.empty())
        return buildings[willBuildBuilding].time;

    int max = 0;
    int preBuild;
    int size = buildings[willBuildBuilding].preBuilded.size();
    for (int i = 0; i < size; ++i) {
        preBuild = buildings[willBuildBuilding].preBuilded[i];
        max = std::max(max, findMinTimeForBuild(buildings, preBuild));
    }

    buildings[willBuildBuilding].time += max;
    return buildings[willBuildBuilding].time;
}
```

### 전체 코드

```c++
#include "iostream"
#include "vector"
#include "algorithm"

struct Building {
    int time;
    std::vector<int> preBuilded;
    bool isCal;
};

std::vector<Building> analyseInputData() {
    int buildingNum, buildedRuleNum;
    std::cin >> buildingNum >> buildedRuleNum;

    std::vector<Building> buildings(buildingNum, Building{0, std::vector<int>(), false});

    for (int i = 0; i < buildingNum; ++i) {
        std::cin >> buildings[i].time;
    }

    for (int i = 0; i < buildedRuleNum; ++i) {
        int building, preBuilded;
        std::cin >> preBuilded >> building;
        buildings[building - 1].preBuilded.push_back(preBuilded - 1);
    }

    return buildings;
}

int findMinTimeForBuild(std::vector<Building> &buildings, int willBuildBuilding) { 
    if (buildings[willBuildBuilding].isCal)
        return buildings[willBuildBuilding].time;
    
    buildings[willBuildBuilding].isCal = true;

    if (buildings[willBuildBuilding].preBuilded.empty())
        return buildings[willBuildBuilding].time;

    int max = 0;
    int preBuild;
    int size = buildings[willBuildBuilding].preBuilded.size();
    for (int i = 0; i < size; ++i) {
        preBuild = buildings[willBuildBuilding].preBuilded[i];
        max = std::max(max, findMinTimeForBuild(buildings, preBuild));
    }

    buildings[willBuildBuilding].time += max;
    return buildings[willBuildBuilding].time;
}

int main() {
    int caseNum;
    std::cin >> caseNum;

    for  (int i = 0; i < caseNum; ++i) {
        int willBuildBuilding;

        std::vector<Building> buildings{std::move(analyseInputData())};
        std::cin >> willBuildBuilding;

        int time = findMinTimeForBuild(buildings, willBuildBuilding - 1);

        std::cout << time << '\n';
    }

    return 0;
}
```



### 오답 노트

| 채점 번호 | 아이디                                                | 문제 번호                                    | 결과                                                   | 메모리 | 시간 | 언어                                                         | 코드 길이 | 제출한 시간                                                  |
| :-------- | :---------------------------------------------------- | :------------------------------------------- | :----------------------------------------------------- | :----- | :--- | :----------------------------------------------------------- | :-------- | :----------------------------------------------------------- |
| 13023179  | [kdpark0723](https://www.acmicpc.net/user/kdpark0723) | [1005](https://www.acmicpc.net/problem/1005) | **맞았습니다!!**                                       | 2648   | 452  | [C++11](https://www.acmicpc.net/source/13023179) / [수정](https://www.acmicpc.net/submit/1005/13023179) | 1803      | [44초 전](https://www.acmicpc.net/status?user_id=kdpark0723&problem_id=1005&from_mine=1#) |
| 13023177  | [kdpark0723](https://www.acmicpc.net/user/kdpark0723) | [1005](https://www.acmicpc.net/problem/1005) | [컴파일 에러](https://www.acmicpc.net/ceinfo/13023177) |        |      | [C++11](https://www.acmicpc.net/source/13023177) / [수정](https://www.acmicpc.net/submit/1005/13023177) | 1802      | [1분 전](https://www.acmicpc.net/status?user_id=kdpark0723&problem_id=1005&from_mine=1#) |
| 13023170  | [kdpark0723](https://www.acmicpc.net/user/kdpark0723) | [1005](https://www.acmicpc.net/problem/1005) | [컴파일 에러](https://www.acmicpc.net/ceinfo/13023170) |        |      | [C++11](https://www.acmicpc.net/source/13023170) / [수정](https://www.acmicpc.net/submit/1005/13023170) | 2470      | [2분 전](https://www.acmicpc.net/status?user_id=kdpark0723&problem_id=1005&from_mine=1#) |
| 13023110  | [kdpark0723](https://www.acmicpc.net/user/kdpark0723) | [1005](https://www.acmicpc.net/problem/1005) | 시간 초과                                              |        |      | [C++11](https://www.acmicpc.net/source/13023110) / [수정](https://www.acmicpc.net/submit/1005/13023110) | 1665      | [8분 전](https://www.acmicpc.net/status?user_id=kdpark0723&problem_id=1005&from_mine=1#) |
| 13023079  | [kdpark0723](https://www.acmicpc.net/user/kdpark0723) | [1005](https://www.acmicpc.net/problem/1005) | 시간 초과                                              |        |      | [C++11](https://www.acmicpc.net/source/13023079) / [수정](https://www.acmicpc.net/submit/1005/13023079) | 1944      | [11분 전](https://www.acmicpc.net/status?user_id=kdpark0723&problem_id=1005&from_mine=1#) |
| 13022675  | [kdpark0723](https://www.acmicpc.net/user/kdpark0723) | [1005](https://www.acmicpc.net/problem/1005) | 시간 초과                                              |        |      | [C++11](https://www.acmicpc.net/source/13022675) / [수정](https://www.acmicpc.net/submit/1005/13022675) | 1922      | [44분 전](https://www.acmicpc.net/status?user_id=kdpark0723&problem_id=1005&from_mine=1#) |
| 13022658  | [kdpark0723](https://www.acmicpc.net/user/kdpark0723) | [1005](https://www.acmicpc.net/problem/1005) | [컴파일 에러](https://www.acmicpc.net/ceinfo/13022658) |        |      | [C++11](https://www.acmicpc.net/source/13022658) / [수정](https://www.acmicpc.net/submit/1005/13022658) | 1900      | [45분 전](https://www.acmicpc.net/status?user_id=kdpark0723&problem_id=1005&from_mine=1#) |
| 13022627  | [kdpark0723](https://www.acmicpc.net/user/kdpark0723) | [1005](https://www.acmicpc.net/problem/1005) | 시간 초과                                              |        |      | [C++11](https://www.acmicpc.net/source/13022627) / [수정](https://www.acmicpc.net/submit/1005/13022627) | 1732      | [46분 전](https://www.acmicpc.net/status?user_id=kdpark0723&problem_id=1005&from_mine=1#) |
| 13022565  | [kdpark0723](https://www.acmicpc.net/user/kdpark0723) | [1005](https://www.acmicpc.net/problem/1005) | 시간 초과                                              |        |      | [C++11](https://www.acmicpc.net/source/13022565) / [수정](https://www.acmicpc.net/submit/1005/13022565) | 1692      |                                                              |

- 첫 구현부터 DP, DFS를 사용하여서 구현하였으나 전에 탐색해야 하는 노드들을 검색할때 노드들을 저장하고 있는 컨테이너의 size() 함수를 호출해서 시간 초과가 나왔다.
- 그후 size()를 루프 전에 호출하여 저장하였으나 그래도 시간 초과가 나왔다.
- 그래서 컨테이너가 비워지지 않았을 경우 루프를 돌고 루프를 돌면서 컨테이너 내부 요소들을 제거하니 성공했다.
- 그후 시간 초과의 원인을 알았는데. DP 구현부분이 삭제 되어 있었다.
- 컴파일 에러는 사용 텍스트 편집기의 저장 충돌 문제로 소스코드가 복사전 손상된 것이 원인중 하나이고 나머지 하나는 나의 부주의다.
- 구현후 적절하게 구현을 수정 하는 시간이 너무 오래 걸린다. 구현전 알고리즘을 어떻게 구현할지 정하고 빠르게 오류없이 구현 하는 것을 연습해야한다.