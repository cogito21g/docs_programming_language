#### 8.3. std::string과 std::string_view

`std::string`과 `std::string_view`는 C++에서 문자열을 처리하는 두 가지 주요 방식입니다. `std::string`은 가변 문자열을 표현하고, `std::string_view`는 상수 문자열을 가리키는 가벼운 객체입니다.

##### std::string

`std::string`은 동적 할당된 가변 길이 문자열을 나타내는 표준 라이브러리 클래스입니다. 다양한 문자열 조작 기능을 제공합니다.

###### 예제

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string s = "Hello, ";
    s += "World!"; // 문자열 결합

    cout << s << endl; // 문자열 출력

    s.replace(0, 5, "Hi"); // 부분 문자열 대체
    cout << s << endl;

    size_t pos = s.find("World"); // 부분 문자열 검색
    if (pos != string::npos) {
        cout << "'World' found at position: " << pos << endl;
    }

    return 0;
}
```

이 예제에서 `std::string`을 사용하여 문자열을 결합하고, 대체하며, 부분 문자열을 검색합니다.

##### std::string_view

`std::string_view`는 상수 문자열을 가리키는 가벼운 객체로, 문자열 복사를 피하고 읽기 전용 문자열 작업을 효율적으로 처리할 수 있습니다.

###### 예제

```cpp
#include <iostream>
#include <string_view>
using namespace std;

void printStringView(string_view sv) {
    cout << sv << endl;
}

int main() {
    string s = "Hello, World!";
    string_view sv = s;

    printStringView(sv); // string_view 출력

    sv.remove_prefix(7); // 앞부분 제거
    printStringView(sv);

    sv.remove_suffix(6); // 뒷부분 제거
    printStringView(sv);

    return 0;
}
```

이 예제에서 `std::string_view`를 사용하여 문자열을 참조하고, 부분 문자열을 제거합니다.

##### std::string과 std::string_view 비교

- **std::string**: 가변 길이 문자열을 나타내며, 동적 할당을 사용하여 문자열을 저장하고 조작합니다.
- **std::string_view**: 상수 문자열을 가리키며, 문자열 복사를 피하고 읽기 전용 문자열 작업을 효율적으로 처리합니다.

###### std::string과 std::string_view 함께 사용하기

```cpp
#include <iostream>
#include <string>
#include <string_view>
using namespace std;

void processString(string_view sv) {
    if (sv.starts_with("Hello")) {
        cout << "Greeting detected." << endl;
    }
    if (sv.ends_with("World!")) {
        cout << "World detected." << endl;
    }
}

int main() {
    string s = "Hello, World!";
    processString(s); // std::string을 std::string_view로 전달

    const char* cstr = "Hello, World!";
    processString(cstr); // C 문자열을 std::string_view로 전달

    return 0;
}
```

이 예제에서 `std::string`과 C 문자열을 `std::string_view`로 전달하여 문자열을 처리합니다.

##### std::string과 std::string_view의 주요 함수

###### std::string의 주요 함수

- `append`: 문자열 결합
- `replace`: 부분 문자열 대체
- `find`: 부분 문자열 검색
- `substr`: 부분 문자열 추출

###### 예제

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string s = "Hello, World!";
    
    s.append(" How are you?"); // 문자열 결합
    cout << s << endl;
    
    s.replace(7, 5, "Universe"); // 부분 문자열 대체
    cout << s << endl;
    
    size_t pos = s.find("Universe"); // 부분 문자열 검색
    if (pos != string::npos) {
        cout << "'Universe' found at position: " << pos << endl;
    }
    
    string sub = s.substr(0, 5); // 부분 문자열 추출
    cout << "Substring: " << sub << endl;

    return 0;
}
```

이 예제에서 `std::string`의 주요 함수를 사용하여 문자열을 조작합니다.

###### std::string_view의 주요 함수

- `remove_prefix`: 앞부분 제거
- `remove_suffix`: 뒷부분 제거
- `substr`: 부분 문자열 추출
- `starts_with`: 특정 접두사 확인
- `ends_with`: 특정 접미사 확인

###### 예제

```cpp
#include <iostream>
#include <string_view>
using namespace std;

int main() {
    string_view sv = "Hello, World!";
    
    sv.remove_prefix(7); // 앞부분 제거
    cout << sv << endl;
    
    sv.remove_suffix(6); // 뒷부분 제거
    cout << sv << endl;

    if (sv.starts_with("W")) {
        cout << "Starts with 'W'" << endl;
    }
    
    if (sv.ends_with("rld")) {
        cout << "Ends with 'rld'" << endl;
    }

    return 0;
}
```

이 예제에서 `std::string_view`의 주요 함수를 사용하여 문자열을 조작합니다.

#### 요약

- **std::string**: 가변 길이 문자열을 나타내며, 동적 할당을 사용하여 문자열을 저장하고 조작합니다.
  - 주요 함수: `append`, `replace`, `find`, `substr`
- **std::string_view**: 상수 문자열을 가리키며, 문자열 복사를 피하고 읽기 전용 문자열 작업을 효율적으로 처리합니다.
  - 주요 함수: `remove_prefix`, `remove_suffix`, `substr`, `starts_with`, `ends_with`
