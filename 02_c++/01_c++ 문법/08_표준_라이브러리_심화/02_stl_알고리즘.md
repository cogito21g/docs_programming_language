#### 8.2. STL 알고리즘 (sort, find, transform 등)

표준 템플릿 라이브러리(STL) 알고리즘은 다양한 데이터 처리 작업을 수행할 수 있는 함수들로 구성되어 있습니다. 이들 알고리즘은 컨테이너와 함께 사용되어 데이터를 정렬, 검색, 변환, 조작하는 데 유용합니다.

##### sort

`std::sort`는 범위를 정렬하는 알고리즘입니다. 이는 기본적으로 오름차순으로 정렬하며, 사용자 정의 비교 함수를 사용할 수 있습니다.

###### 예제

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    vector<int> vec = {5, 2, 9, 1, 5, 6};

    // 기본 오름차순 정렬
    sort(vec.begin(), vec.end());

    for (int n : vec) {
        cout << n << " ";
    }
    cout << endl;

    // 내림차순 정렬
    sort(vec.begin(), vec.end(), greater<int>());

    for (int n : vec) {
        cout << n << " ";
    }
    cout << endl;

    return 0;
}
```

이 예제에서 `std::sort`를 사용하여 벡터를 오름차순과 내림차순으로 정렬합니다.

##### find

`std::find`는 범위에서 특정 값을 찾는 알고리즘입니다. 값을 찾으면 해당 위치의 반복자를 반환하고, 찾지 못하면 범위의 끝 반복자를 반환합니다.

###### 예제

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    vector<int> vec = {1, 2, 3, 4, 5};

    // 값 3을 찾기
    auto it = find(vec.begin(), vec.end(), 3);
    if (it != vec.end()) {
        cout << "Found: " << *it << endl;
    } else {
        cout << "Not found" << endl;
    }

    // 값 6을 찾기
    it = find(vec.begin(), vec.end(), 6);
    if (it != vec.end()) {
        cout << "Found: " << *it << endl;
    } else {
        cout << "Not found" << endl;
    }

    return 0;
}
```

이 예제에서 `std::find`를 사용하여 벡터에서 값을 검색합니다.

##### transform

`std::transform`은 범위의 각 요소에 대해 지정된 변환 함수를 적용하여 결과를 다른 범위에 저장하는 알고리즘입니다.

###### 예제

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    vector<int> vec = {1, 2, 3, 4, 5};
    vector<int> result(vec.size());

    // 각 요소를 제곱하여 result 벡터에 저장
    transform(vec.begin(), vec.end(), result.begin(), [](int n) { return n * n; });

    for (int n : result) {
        cout << n << " ";
    }
    cout << endl;

    return 0;
}
```

이 예제에서 `std::transform`을 사용하여 벡터의 각 요소를 제곱하고, 결과를 다른 벡터에 저장합니다.

##### for_each

`std::for_each`는 범위의 각 요소에 대해 지정된 함수를 적용하는 알고리즘입니다.

###### 예제

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    vector<int> vec = {1, 2, 3, 4, 5};

    // 각 요소를 출력
    for_each(vec.begin(), vec.end(), [](int n) {
        cout << n << " ";
    });
    cout << endl;

    return 0;
}
```

이 예제에서 `std::for_each`를 사용하여 벡터의 각 요소를 출력합니다.

##### accumulate

`std::accumulate`는 범위의 요소를 합산하는 알고리즘입니다.

###### 예제

```cpp
#include <iostream>
#include <vector>
#include <numeric>
using namespace std;

int main() {
    vector<int> vec = {1, 2, 3, 4, 5};

    // 벡터 요소의 합
    int sum = accumulate(vec.begin(), vec.end(), 0);
    cout << "Sum: " << sum << endl;

    // 벡터 요소의 곱
    int product = accumulate(vec.begin(), vec.end(), 1, multiplies<int>());
    cout << "Product: " << product << endl;

    return 0;
}
```

이 예제에서 `std::accumulate`를 사용하여 벡터의 요소를 합산하고, 곱을 계산합니다.

##### count_if

`std::count_if`는 범위에서 조건을 만족하는 요소의 개수를 셉니다.

###### 예제

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    vector<int> vec = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};

    // 짝수 개수 세기
    int even_count = count_if(vec.begin(), vec.end(), [](int n) {
        return n % 2 == 0;
    });
    cout << "Even count: " << even_count << endl;

    return 0;
}
```

이 예제에서 `std::count_if`를 사용하여 벡터에서 짝수의 개수를 셉니다.

#### 요약

- **sort**: 범위를 정렬하는 알고리즘입니다.
- **find**: 범위에서 특정 값을 찾는 알고리즘입니다.
- **transform**: 범위의 각 요소에 대해 지정된 변환 함수를 적용하는 알고리즘입니다.
- **for_each**: 범위의 각 요소에 대해 지정된 함수를 적용하는 알고리즘입니다.
- **accumulate**: 범위의 요소를 합산하는 알고리즘입니다.
- **count_if**: 범위에서 조건을 만족하는 요소의 개수를 세는 알고리즘입니다.
