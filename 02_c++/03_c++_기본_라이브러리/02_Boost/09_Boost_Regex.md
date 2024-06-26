#### 2.9. Boost.Regex

Boost.Regex 라이브러리는 C++에서 정규 표현식을 사용하여 문자열을 검색, 일치, 대체할 수 있는 기능을 제공합니다. 이 라이브러리는 C++11에서 도입된 `std::regex`의 기반이 되었습니다.

##### Boost.Regex 설치
Boost 라이브러리가 설치되어 있어야 합니다. 설치 방법은 이전 섹션을 참조하세요.

##### 주요 클래스 및 함수

- `boost::regex`: 정규 표현식을 정의하는 클래스.
- `boost::smatch`: 문자열 매칭 결과를 저장하는 클래스.
- `boost::regex_search`: 문자열에서 정규 표현식과 일치하는 부분을 검색하는 함수.
- `boost::regex_match`: 문자열이 정규 표현식과 완전히 일치하는지 확인하는 함수.
- `boost::regex_replace`: 정규 표현식과 일치하는 부분을 다른 문자열로 대체하는 함수.

##### 예제 1: 기본적인 정규 표현식 검색

```cpp
#include <iostream>
#include <boost/regex.hpp>

int main() {
    std::string text = "The quick brown fox jumps over the lazy dog.";
    boost::regex expr("\\b\\w{4}\\b"); // 길이가 4인 단어를 검색하는 정규 표현식
    boost::smatch match;

    if (boost::regex_search(text, match, expr)) {
        std::cout << "Found match: " << match[0] << std::endl;
    } else {
        std::cout << "No match found" << std::endl;
    }

    return 0;
}
```

##### 예제 2: 정규 표현식 일치

```cpp
#include <iostream>
#include <boost/regex.hpp>

int main() {
    std::string text = "abcd";
    boost::regex expr("^[a-z]{4}$"); // 소문자 4글자로 구성된 문자열

    if (boost::regex_match(text, expr)) {
        std::cout << "Text matches the regular expression" << std::endl;
    } else {
        std::cout << "Text does not match the regular expression" << std::endl;
    }

    return 0;
}
```

##### 예제 3: 정규 표현식 대체

```cpp
#include <iostream>
#include <boost/regex.hpp>

int main() {
    std::string text = "The quick brown fox jumps over the lazy dog.";
    boost::regex expr("\\b\\w{4}\\b"); // 길이가 4인 단어를 검색하는 정규 표현식
    std::string replacement = "****";

    std::string result = boost::regex_replace(text, expr, replacement);
    std::cout << "Result: " << result << std::endl;

    return 0;
}
```

##### 예제 4: 정규 표현식 매칭 결과 반복

```cpp
#include <iostream>
#include <boost/regex.hpp>

int main() {
    std::string text = "The quick brown fox jumps over the lazy dog.";
    boost::regex expr("\\b\\w{4}\\b"); // 길이가 4인 단어를 검색하는 정규 표현식
    boost::sregex_iterator begin(text.begin(), text.end(), expr);
    boost::sregex_iterator end;

    std::cout << "Matches: ";
    for (auto it = begin; it != end; ++it) {
        std::cout << it->str() << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

##### 예제 5: 그룹화된 정규 표현식

```cpp
#include <iostream>
#include <boost/regex.hpp>

int main() {
    std::string text = "My email is example@example.com.";
    boost::regex expr("(\\w+)@(\\w+\\.\\w+)");
    boost::smatch match;

    if (boost::regex_search(text, match, expr)) {
        std::cout << "Full match: " << match[0] << std::endl;
        std::cout << "Username: " << match[1] << std::endl;
        std::cout << "Domain: " << match[2] << std::endl;
    } else {
        std::cout << "No match found" << std::endl;
    }

    return 0;
}
```

##### 예제 6: 정규 표현식을 사용한 문자열 분할

```cpp
#include <iostream>
#include <boost/regex.hpp>
#include <boost/algorithm/string/regex.hpp>

int main() {
    std::string text = "one,two,three,four";
    boost::regex expr(",");
    std::vector<std::string> tokens;

    boost::algorithm::split_regex(tokens, text, expr);

    std::cout << "Tokens: ";
    for (const auto& token : tokens) {
        std::cout << token << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

이 예제에서는 Boost.Regex를 사용하여 쉼표로 구분된 문자열을 개별 토큰으로 분할합니다.

##### 예제 7: 정규 표현식 옵션 사용

```cpp
#include <iostream>
#include <boost/regex.hpp>

int main() {
    std::string text = "The Quick Brown Fox";
    boost::regex expr("\\bthe\\b", boost::regex::icase); // 대소문자 구분 없는 검색

    if (boost::regex_search(text, expr)) {
        std::cout << "Found match (case insensitive)" << std::endl;
    } else {
        std::cout << "No match found" << std::endl;
    }

    return 0;
}
```

이 예제에서는 `boost::regex::icase` 옵션을 사용하여 대소문자를 구분하지 않고 검색하는 방법을 보여줍니다.
