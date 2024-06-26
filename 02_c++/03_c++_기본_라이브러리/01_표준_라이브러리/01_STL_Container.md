### 1. C++ 표준 라이브러리 (C++ Standard Library)

#### 1.1. STL 컨테이너 (vector, list, map, set 등)

C++ 표준 라이브러리의 컨테이너는 데이터를 효율적으로 저장하고 조작할 수 있는 다양한 자료구조를 제공합니다. 여기에는 `vector`, `list`, `map`, `set` 등이 포함됩니다.

##### Vector
`vector`는 동적 배열을 제공하는 컨테이너로, 요소를 순차적으로 저장합니다. `vector`는 배열과 달리 크기를 동적으로 조절할 수 있으며, 임의 접근이 빠릅니다.

**사용법 예제**:

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> numbers;
    
    // 요소 추가
    numbers.push_back(1);
    numbers.push_back(2);
    numbers.push_back(3);

    // 요소 접근
    for (size_t i = 0; i < numbers.size(); ++i) {
        std::cout << numbers[i] << " ";
    }
    std::cout << std::endl;

    // 요소 제거
    numbers.pop_back();

    // 요소 접근
    for (const auto& num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

##### List
`list`는 이중 연결 리스트를 제공하는 컨테이너로, 삽입과 삭제가 리스트의 임의 위치에서 효율적으로 수행됩니다. `list`는 순차 접근만 가능합니다.

**사용법 예제**:

```cpp
#include <iostream>
#include <list>

int main() {
    std::list<int> numbers;

    // 요소 추가
    numbers.push_back(1);
    numbers.push_back(2);
    numbers.push_back(3);

    // 요소 접근
    for (auto it = numbers.begin(); it != numbers.end(); ++it) {
        std::cout << *it << " ";
    }
    std::cout << std::endl;

    // 요소 제거
    numbers.pop_back();

    // 요소 접근
    for (const auto& num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

##### Map
`map`은 키와 값의 쌍으로 이루어진 연관 컨테이너로, 키를 이용한 빠른 검색을 지원합니다. `map`은 키를 기준으로 자동으로 정렬됩니다.

**사용법 예제**:

```cpp
#include <iostream>
#include <map>

int main() {
    std::map<std::string, int> ageMap;

    // 요소 추가
    ageMap["Alice"] = 30;
    ageMap["Bob"] = 25;
    ageMap["Charlie"] = 35;

    // 요소 접근
    for (const auto& pair : ageMap) {
        std::cout << pair.first << ": " << pair.second << std::endl;
    }

    // 요소 검색
    if (ageMap.find("Alice") != ageMap.end()) {
        std::cout << "Alice is " << ageMap["Alice"] << " years old." << std::endl;
    }

    return 0;
}
```

##### Set
`set`은 중복되지 않는 요소의 집합을 저장하는 컨테이너로, 각 요소는 유일해야 합니다. `set`은 요소를 자동으로 정렬하며, 빠른 검색을 지원합니다.

**사용법 예제**:

```cpp
#include <iostream>
#include <set>

int main() {
    std::set<int> uniqueNumbers;

    // 요소 추가
    uniqueNumbers.insert(1);
    uniqueNumbers.insert(2);
    uniqueNumbers.insert(3);
    uniqueNumbers.insert(2); // 중복 요소는 추가되지 않음

    // 요소 접근
    for (const auto& num : uniqueNumbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    // 요소 검색
    if (uniqueNumbers.find(2) != uniqueNumbers.end()) {
        std::cout << "2 is in the set." << std::endl;
    }

    return 0;
}
```
