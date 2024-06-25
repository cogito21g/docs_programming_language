#### 6.6. 초기화된 if/switch

C++17에서 도입된 초기화된 if와 switch 문은 조건문을 더 간결하고 명확하게 작성할 수 있도록 합니다. C++20에서도 이 기능이 그대로 사용됩니다.

##### 초기화된 if 문

초기화된 if 문은 조건을 검사하기 전에 변수 초기화를 할 수 있게 합니다. 이를 통해 조건문 내에서 사용될 변수를 초기화하고, 그 범위를 조건문 블록으로 제한할 수 있습니다.

###### 기본 사용법

```cpp
#include <iostream>
#include <optional>
using namespace std;

int main() {
    optional<int> opt = 42;

    if (auto value = opt; value.has_value()) {
        cout << "Value: " << value.value() << endl;
    } else {
        cout << "No value" << endl;
    }

    return 0;
}
```

이 예제에서 `if` 문 안에서 `value` 변수를 초기화하고, 그 변수를 조건문 블록 내에서 사용합니다.

##### 초기화된 switch 문

초기화된 switch 문은 조건을 검사하기 전에 변수 초기화를 할 수 있게 합니다. 이를 통해 switch 문 내에서 사용될 변수를 초기화하고, 그 범위를 switch 문 블록으로 제한할 수 있습니다.

###### 기본 사용법

```cpp
#include <iostream>
using namespace std;

int main() {
    int x = 2;

    switch (auto value = x * 2; value) {
        case 2:
            cout << "Value is 2" << endl;
            break;
        case 4:
            cout << "Value is 4" << endl;
            break;
        case 6:
            cout << "Value is 6" << endl;
            break;
        default:
            cout << "Value is something else" << endl;
            break;
    }

    return 0;
}
```

이 예제에서 `switch` 문 안에서 `value` 변수를 초기화하고, 그 변수를 `switch` 문 블록 내에서 사용합니다.

##### 실용적인 예제

초기화된 if/switch 문은 복잡한 조건문을 보다 명확하고 간결하게 작성하는 데 유용합니다.

###### 예제: 초기화된 if 문을 활용한 파일 읽기

```cpp
#include <iostream>
#include <fstream>
#include <string>
using namespace std;

int main() {
    if (ifstream file("example.txt"); file.is_open()) {
        string line;
        while (getline(file, line)) {
            cout << line << endl;
        }
    } else {
        cout << "Failed to open file" << endl;
    }

    return 0;
}
```

이 예제에서 `ifstream` 객체를 `if` 문 안에서 초기화하고, 파일이 열렸는지 확인한 후 파일 내용을 읽습니다.

###### 예제: 초기화된 switch 문을 활용한 메뉴 선택

```cpp
#include <iostream>
using namespace std;

int main() {
    int choice;
    cout << "Enter your choice (1-3): ";
    cin >> choice;

    switch (auto opt = choice; opt) {
        case 1:
            cout << "You chose option 1" << endl;
            break;
        case 2:
            cout << "You chose option 2" << endl;
            break;
        case 3:
            cout << "You chose option 3" << endl;
            break;
        default:
            cout << "Invalid choice" << endl;
            break;
    }

    return 0;
}
```

이 예제에서 `choice` 변수를 `switch` 문 안에서 초기화하고, 그 변수를 사용하여 사용자 선택에 따라 다른 메시지를 출력합니다.

##### 초기화된 if/switch 문 사용의 장점

- **가독성 향상**: 초기화된 변수가 조건문 블록 내에 제한되어 코드의 가독성이 높아집니다.
- **범위 제한**: 초기화된 변수의 범위가 조건문 블록 내로 제한되어, 코드의 명확성과 유지보수성이 향상됩니다.
- **코드 간결성**: 초기화와 조건 검사를 한 줄로 작성하여 코드가 더 간결해집니다.

#### 요약

- **초기화된 if 문**: 조건문 내에서 변수를 초기화하고, 그 범위를 조건문 블록으로 제한합니다.
  - 기본 사용법: `if (auto var = init; condition) { ... }`
- **초기화된 switch 문**: switch 문 내에서 변수를 초기화하고, 그 범위를 switch 문 블록으로 제한합니다.
  - 기본 사용법: `switch (auto var = init; value) { ... }`
- **장점**: 가독성 향상, 범위 제한, 코드 간결성.
