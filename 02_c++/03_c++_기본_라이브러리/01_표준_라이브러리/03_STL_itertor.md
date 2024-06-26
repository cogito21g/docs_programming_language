#### 1.3. STL 반복자 (Iterators)

반복자는 STL 컨테이너의 요소를 순회하고 조작하기 위한 일반화된 방법을 제공합니다. 반복자는 포인터와 유사한 역할을 하며, 다양한 유형이 있습니다: 입력 반복자, 출력 반복자, 전방 반복자, 양방향 반복자, 임의 접근 반복자.

##### 입력 반복자 (Input Iterators)
입력 반복자는 순차적으로 데이터를 읽을 수 있습니다. 주로 읽기 전용 반복자입니다.

**사용법 예제**:

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> numbers = {1, 2, 3, 4, 5};

    // 입력 반복자를 사용하여 요소 출력
    for (std::vector<int>::iterator it = numbers.begin(); it != numbers.end(); ++it) {
        std::cout << *it << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

##### 출력 반복자 (Output Iterators)
출력 반복자는 데이터를 순차적으로 쓸 수 있습니다. 주로 쓰기 전용 반복자입니다.

**사용법 예제**:

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

int main() {
    std::vector<int> numbers(5);

    // 출력 반복자를 사용하여 요소 초기화
    std::fill(numbers.begin(), numbers.end(), 10);

    for (const auto& num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

##### 전방 반복자 (Forward Iterators)
전방 반복자는 입력 반복자와 출력 반복자의 기능을 모두 가지며, 단방향으로만 이동할 수 있습니다.

**사용법 예제**:

```cpp
#include <iostream>
#include <forward_list>

int main() {
    std::forward_list<int> numbers = {1, 2, 3, 4, 5};

    // 전방 반복자를 사용하여 요소 출력
    for (auto it = numbers.begin(); it != numbers.end(); ++it) {
        std::cout << *it << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

##### 양방향 반복자 (Bidirectional Iterators)
양방향 반복자는 전방 반복자의 기능을 가지며, 양방향으로 이동할 수 있습니다.

**사용법 예제**:

```cpp
#include <iostream>
#include <list>

int main() {
    std::list<int> numbers = {1, 2, 3, 4, 5};

    // 양방향 반복자를 사용하여 요소 출력
    for (auto it = numbers.begin(); it != numbers.end(); ++it) {
        std::cout << *it << " ";
    }
    std::cout << std::endl;

    // 역방향으로 요소 출력
    for (auto it = numbers.rbegin(); it != numbers.rend(); ++it) {
        std::cout << *it << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

##### 임의 접근 반복자 (Random Access Iterators)
임의 접근 반복자는 양방향 반복자의 기능을 가지며, 임의의 위치로 직접 이동할 수 있습니다. 벡터와 데크에서 주로 사용됩니다.

**사용법 예제**:

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> numbers = {1, 2, 3, 4, 5};

    // 임의 접근 반복자를 사용하여 요소 접근
    for (size_t i = 0; i < numbers.size(); ++i) {
        std::cout << numbers[i] << " ";
    }
    std::cout << std::endl;

    // 임의 위치로 이동
    auto it = numbers.begin();
    std::advance(it, 3);  // 3번째 요소로 이동
    std::cout << "Fourth element: " << *it << std::endl;

    return 0;
}
```

##### 기타 반복자 유틸리티
- `std::back_inserter`: 컨테이너의 끝에 요소를 삽입하는 출력 반복자.
- `std::reverse_iterator`: 컨테이너의 역방향 반복자.
- `std::move_iterator`: 반복자를 이동 반복자로 변환하는 유틸리티.

**예제 코드 (std::back_inserter)**:

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

int main() {
    std::vector<int> numbers = {1, 2, 3};
    std::vector<int> more_numbers = {4, 5, 6};

    // back_inserter를 사용하여 요소 추가
    std::copy(more_numbers.begin(), more_numbers.end(), std::back_inserter(numbers));

    for (const auto& num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    return 0;
}
```
