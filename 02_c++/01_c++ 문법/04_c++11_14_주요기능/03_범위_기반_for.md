#### 4.3. 범위 기반 for 루프

범위 기반 for 루프는 C++11에서 도입된 기능으로, 컨테이너나 배열의 모든 요소를 편리하게 순회할 수 있게 합니다. 이를 통해 반복문을 간결하게 작성할 수 있습니다.

##### 기본 사용법

범위 기반 for 루프의 기본 문법은 다음과 같습니다:

```cpp
for (declaration : range) {
    // 반복할 코드
}
```

- **declaration**: 각 요소를 참조할 변수의 선언입니다.
- **range**: 순회할 컨테이너나 배열입니다.

###### 예제

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> vec = {1, 2, 3, 4, 5};

    // 범위 기반 for 루프를 사용하여 벡터의 모든 요소를 출력
    for (int n : vec) {
        cout << n << " ";
    }
    cout << endl;

    return 0;
}
```

이 예제에서 범위 기반 for 루프는 `vec` 벡터의 모든 요소를 순회하며, 각 요소를 출력합니다.

##### 참조와 const를 사용한 범위 기반 for 루프

범위 기반 for 루프는 참조와 const를 함께 사용하여 요소를 수정하거나 읽기 전용으로 순회할 수 있습니다.

###### 참조를 사용한 예제

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> vec = {1, 2, 3, 4, 5};

    // 참조를 사용하여 벡터의 각 요소를 2배로 증가
    for (int &n : vec) {
        n *= 2;
    }

    // 수정된 벡터의 모든 요소를 출력
    for (int n : vec) {
        cout << n << " ";
    }
    cout << endl;

    return 0;
}
```

이 예제에서 범위 기반 for 루프는 참조를 사용하여 `vec` 벡터의 각 요소를 수정합니다. 요소를 2배로 증가시킨 후, 수정된 벡터의 모든 요소를 출력합니다.

###### const 참조를 사용한 예제

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> vec = {1, 2, 3, 4, 5};

    // const 참조를 사용하여 벡터의 모든 요소를 읽기 전용으로 출력
    for (const int &n : vec) {
        cout << n << " ";
    }
    cout << endl;

    return 0;
}
```

이 예제에서 범위 기반 for 루프는 `const` 참조를 사용하여 `vec` 벡터의 요소를 읽기 전용으로 순회하며, 각 요소를 출력합니다.

##### 배열과 함께 사용

범위 기반 for 루프는 STL 컨테이너뿐만 아니라 배열과 함께 사용할 수 있습니다.

###### 배열 예제

```cpp
#include <iostream>
using namespace std;

int main() {
    int arr[] = {1, 2, 3, 4, 5};

    // 범위 기반 for 루프를 사용하여 배열의 모든 요소를 출력
    for (int n : arr) {
        cout << n << " ";
    }
    cout << endl;

    return 0;
}
```

이 예제에서 범위 기반 for 루프는 `arr` 배열의 모든 요소를 순회하며, 각 요소를 출력합니다.

##### 범위 기반 for 루프의 장점

- **간결성**: 전통적인 for 루프에 비해 코드가 간결하고 명확합니다.
- **안전성**: 인덱스와 반복자 관리를 자동으로 처리하므로, 인덱스 범위 초과와 같은 오류를 방지할 수 있습니다.
- **가독성**: 코드가 읽기 쉽고 의도를 명확하게 나타냅니다.

#### 요약

- **범위 기반 for 루프**: 컨테이너나 배열의 모든 요소를 편리하게 순회할 수 있습니다.
  - 기본 문법: `for (declaration : range)`
  - 참조와 const를 사용하여 요소를 수정하거나 읽기 전용으로 순회할 수 있습니다.
  - 배열과 함께 사용할 수 있습니다.
- **장점**: 코드의 간결성, 안전성, 가독성을 높입니다.
