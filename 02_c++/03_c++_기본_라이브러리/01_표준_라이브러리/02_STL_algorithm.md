#### 1.2. STL 알고리즘 (sort, find, transform 등)

C++ 표준 라이브러리의 알고리즘은 컨테이너에 저장된 데이터를 조작하고 처리하기 위한 다양한 함수를 제공합니다. 여기에는 정렬, 검색, 변환 등이 포함됩니다. 이들은 `<algorithm>` 헤더에 정의되어 있습니다.

##### Sort
`sort`는 컨테이너의 요소를 정렬하는 알고리즘입니다. 기본적으로 오름차순으로 정렬하며, 사용자 정의 비교 함수를 사용하여 정렬 순서를 변경할 수 있습니다.

**사용법 예제**:

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

int main() {
    std::vector<int> numbers = {5, 2, 9, 1, 5, 6};

    // 오름차순 정렬
    std::sort(numbers.begin(), numbers.end());

    std::cout << "Sorted numbers: ";
    for (const auto& num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    // 내림차순 정렬
    std::sort(numbers.begin(), numbers.end(), std::greater<int>());

    std::cout << "Reverse sorted numbers: ";
    for (const auto& num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

##### Find
`find`는 컨테이너에서 특정 값을 검색하는 알고리즘입니다. 검색된 첫 번째 요소의 반복자를 반환하며, 검색 결과가 없으면 `end` 반복자를 반환합니다.

**사용법 예제**:

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

int main() {
    std::vector<int> numbers = {5, 2, 9, 1, 5, 6};

    // 값 검색
    auto it = std::find(numbers.begin(), numbers.end(), 9);

    if (it != numbers.end()) {
        std::cout << "Found 9 at position: " << std::distance(numbers.begin(), it) << std::endl;
    } else {
        std::cout << "9 not found" << std::endl;
    }

    return 0;
}
```

##### Transform
`transform`은 컨테이너의 각 요소에 함수를 적용하여 새로운 컨테이너를 생성하는 알고리즘입니다.

**사용법 예제**:

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

int main() {
    std::vector<int> numbers = {1, 2, 3, 4, 5};
    std::vector<int> squares(numbers.size());

    // 각 요소의 제곱 계산
    std::transform(numbers.begin(), numbers.end(), squares.begin(), [](int x) { return x * x; });

    std::cout << "Squares: ";
    for (const auto& num : squares) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

##### Other Useful Algorithms
다른 유용한 알고리즘으로는 `for_each`, `count`, `accumulate`, `copy`, `remove`, `replace` 등이 있습니다.

- `for_each`: 컨테이너의 각 요소에 대해 주어진 함수를 호출합니다.
- `count`: 컨테이너에서 특정 값의 개수를 셉니다.
- `accumulate`: 컨테이너의 모든 요소를 누적 합산합니다.
- `copy`: 한 컨테이너의 내용을 다른 컨테이너에 복사합니다.
- `remove`: 컨테이너에서 특정 값을 제거합니다.
- `replace`: 컨테이너에서 특정 값을 다른 값으로 교체합니다.

**예제 코드 (accumulate)**:

```cpp
#include <iostream>
#include <vector>
#include <numeric>

int main() {
    std::vector<int> numbers = {1, 2, 3, 4, 5};

    // 누적 합 계산
    int sum = std::accumulate(numbers.begin(), numbers.end(), 0);

    std::cout << "Sum of numbers: " << sum << std::endl;

    return 0;
}
```
