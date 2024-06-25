#### 7.5. 범위 기반 for 루프 개선

C++23에서는 범위 기반 for 루프가 개선되어, 더 유연하고 직관적인 반복문 작성이 가능해졌습니다. 이 개선은 특히 다양한 컨테이너와 범위 객체를 쉽게 순회할 수 있도록 합니다.

##### 기본 사용법

범위 기반 for 루프는 C++11에서 도입되어, 컨테이너와 배열을 간편하게 순회할 수 있게 해줍니다. C++23에서는 이를 더 직관적이고 유연하게 사용할 수 있도록 개선되었습니다.

###### 기본 예제

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> vec = {1, 2, 3, 4, 5};

    // 기본 범위 기반 for 루프
    for (int n : vec) {
        cout << n << " ";
    }
    cout << endl;

    return 0;
}
```

이 예제에서 `vec` 벡터의 모든 요소를 범위 기반 for 루프를 사용하여 출력합니다.

##### 범위 기반 for 루프와 구조화된 바인딩

C++23에서는 범위 기반 for 루프와 구조화된 바인딩을 결합하여 사용할 수 있습니다. 이를 통해 복합 데이터 타입을 쉽게 분해하고 순회할 수 있습니다.

###### 예제: 구조화된 바인딩과 결합

```cpp
#include <iostream>
#include <map>
using namespace std;

int main() {
    map<int, string> m = {{1, "one"}, {2, "two"}, {3, "three"}};

    // 구조화된 바인딩을 사용한 범위 기반 for 루프
    for (const auto& [key, value] : m) {
        cout << key << ": " << value << endl;
    }

    return 0;
}
```

이 예제에서 `map`의 각 요소를 구조화된 바인딩을 사용하여 키와 값을 분해하고 출력합니다.

##### 범위 기반 for 루프와 컨테이너 어댑터

C++23에서는 범위 기반 for 루프와 컨테이너 어댑터를 결합하여 사용할 수 있습니다. 이를 통해 다양한 컨테이너와 어댑터를 쉽게 순회할 수 있습니다.

###### 예제: 컨테이너 어댑터와 결합

```cpp
#include <iostream>
#include <stack>
using namespace std;

int main() {
    stack<int> s;
    s.push(1);
    s.push(2);
    s.push(3);

    // stack을 직접 순회할 수 없으므로 vector에 복사하여 순회
    vector<int> vec;
    while (!s.empty()) {
        vec.push_back(s.top());
        s.pop();
    }

    // 범위 기반 for 루프를 사용하여 벡터 순회
    for (int n : vec) {
        cout << n << " ";
    }
    cout << endl;

    return 0;
}
```

이 예제에서 `stack`의 요소를 벡터로 복사한 후, 범위 기반 for 루프를 사용하여 벡터를 순회합니다.

##### 범위 기반 for 루프와 std::ranges

C++23에서는 `std::ranges` 라이브러리와 결합하여 범위 기반 for 루프를 사용할 수 있습니다. 이를 통해 범위 기반 알고리즘과 변환을 쉽게 수행할 수 있습니다.

###### 예제: std::ranges와 결합

```cpp
#include <iostream>
#include <vector>
#include <ranges>
using namespace std;
namespace ranges = std::ranges;

int main() {
    vector<int> vec = {1, 2, 3, 4, 5};

    // ranges::views::transform을 사용하여 각 요소를 2배로 변환
    auto transformed = vec | ranges::views::transform([](int n) { return n * 2; });

    // 범위 기반 for 루프를 사용하여 변환된 요소 출력
    for (int n : transformed) {
        cout << n << " ";
    }
    cout << endl;

    return 0;
}
```

이 예제에서 `std::ranges::views::transform`을 사용하여 벡터의 각 요소를 변환하고, 범위 기반 for 루프를 사용하여 변환된 요소를 출력합니다.

##### 범위 기반 for 루프의 장점

- **가독성 향상**: 범위 기반 for 루프는 코드의 가독성을 높여줍니다.
- **유연성**: 다양한 컨테이너와 범위 객체를 쉽게 순회할 수 있습니다.
- **구조화된 바인딩**: 복합 데이터 타입을 쉽게 분해하고 순회할 수 있습니다.

#### 요약

- **범위 기반 for 루프 개선**: C++23에서는 범위 기반 for 루프의 사용이 더 유연하고 직관적으로 개선되었습니다.
  - **구조화된 바인딩**: 범위 기반 for 루프와 결합하여 복합 데이터 타입을 분해하고 순회할 수 있습니다.
  - **컨테이너 어댑터**: 다양한 컨테이너 어댑터와 결합하여 사용할 수 있습니다.
  - **std::ranges**: `std::ranges` 라이브러리와 결합하여 범위 기반 알고리즘과 변환을 쉽게 수행할 수 있습니다.
- **장점**: 코드의 가독성, 유연성, 구조화된 바인딩을 통해 복합 데이터 타입을 쉽게 처리할 수 있습니다.
