### 8. 표준 라이브러리 심화

#### 8.1. STL 컨테이너 (vector, list, map, set 등)

표준 템플릿 라이브러리(STL)는 C++의 핵심 구성 요소로, 다양한 컨테이너, 알고리즘 및 반복자를 제공합니다. 주요 컨테이너로는 `vector`, `list`, `map`, `set` 등이 있습니다.

##### vector

`vector`는 동적 배열을 제공하는 컨테이너로, 임의 접근과 효율적인 요소 추가/삭제를 지원합니다.

###### 예제

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> vec = {1, 2, 3, 4, 5};
    
    vec.push_back(6); // 요소 추가
    vec.pop_back();   // 요소 제거

    for (int n : vec) {
        cout << n << " ";
    }
    cout << endl;

    return 0;
}
```

이 예제에서 `vector`를 사용하여 요소를 추가하고 제거한 후, 모든 요소를 출력합니다.

##### list

`list`는 이중 연결 리스트를 제공하는 컨테이너로, 임의의 위치에서 효율적인 삽입 및 삭제를 지원합니다.

###### 예제

```cpp
#include <iostream>
#include <list>
using namespace std;

int main() {
    list<int> lst = {1, 2, 3, 4, 5};
    
    lst.push_back(6); // 요소 추가
    lst.pop_back();   // 요소 제거

    for (int n : lst) {
        cout << n << " ";
    }
    cout << endl;

    return 0;
}
```

이 예제에서 `list`를 사용하여 요소를 추가하고 제거한 후, 모든 요소를 출력합니다.

##### map

`map`은 키-값 쌍을 저장하는 연관 컨테이너로, 키를 기준으로 자동 정렬됩니다. 삽입, 삭제 및 조회 연산이 로그 시간 복잡도를 가집니다.

###### 예제

```cpp
#include <iostream>
#include <map>
using namespace std;

int main() {
    map<int, string> m;
    m[1] = "one";
    m[2] = "two";
    m[3] = "three";

    for (const auto& [key, value] : m) {
        cout << key << ": " << value << endl;
    }

    return 0;
}
```

이 예제에서 `map`을 사용하여 키-값 쌍을 저장하고, 모든 쌍을 출력합니다.

##### set

`set`은 중복되지 않는 고유한 값을 저장하는 연관 컨테이너로, 값은 자동으로 정렬됩니다. 삽입, 삭제 및 조회 연산이 로그 시간 복잡도를 가집니다.

###### 예제

```cpp
#include <iostream>
#include <set>
using namespace std;

int main() {
    set<int> s = {1, 2, 3, 4, 5};
    
    s.insert(6); // 요소 추가
    s.erase(3);  // 요소 제거

    for (int n : s) {
        cout << n << " ";
    }
    cout << endl;

    return 0;
}
```

이 예제에서 `set`을 사용하여 요소를 추가하고 제거한 후, 모든 요소를 출력합니다.

##### unordered_map

`unordered_map`은 해시 테이블을 사용하여 키-값 쌍을 저장하는 컨테이너로, 평균적으로 상수 시간 복잡도로 삽입, 삭제 및 조회를 수행합니다.

###### 예제

```cpp
#include <iostream>
#include <unordered_map>
using namespace std;

int main() {
    unordered_map<int, string> um;
    um[1] = "one";
    um[2] = "two";
    um[3] = "three";

    for (const auto& [key, value] : um) {
        cout << key << ": " << value << endl;
    }

    return 0;
}
```

이 예제에서 `unordered_map`을 사용하여 키-값 쌍을 저장하고, 모든 쌍을 출력합니다.

##### unordered_set

`unordered_set`은 해시 테이블을 사용하여 고유한 값을 저장하는 컨테이너로, 평균적으로 상수 시간 복잡도로 삽입, 삭제 및 조회를 수행합니다.

###### 예제

```cpp
#include <iostream>
#include <unordered_set>
using namespace std;

int main() {
    unordered_set<int> us = {1, 2, 3, 4, 5};
    
    us.insert(6); // 요소 추가
    us.erase(3);  // 요소 제거

    for (int n : us) {
        cout << n << " ";
    }
    cout << endl;

    return 0;
}
```

이 예제에서 `unordered_set`을 사용하여 요소를 추가하고 제거한 후, 모든 요소를 출력합니다.

#### 요약

- **vector**: 동적 배열을 제공하며, 임의 접근과 효율적인 요소 추가/삭제를 지원합니다.
- **list**: 이중 연결 리스트를 제공하며, 임의의 위치에서 효율적인 삽입 및 삭제를 지원합니다.
- **map**: 키-값 쌍을 저장하고 키를 기준으로 자동 정렬되는 연관 컨테이너입니다.
- **set**: 중복되지 않는 고유한 값을 저장하고 자동으로 정렬되는 연관 컨테이너입니다.
- **unordered_map**: 해시 테이블을 사용하여 키-값 쌍을 저장하며 평균적으로 상수 시간 복잡도로 동작합니다.
- **unordered_set**: 해시 테이블을 사용하여 고유한 값을 저장하며 평균적으로 상수 시간 복잡도로 동작합니다.
