#### 2.10. Boost.Random

Boost.Random 라이브러리는 C++ 프로그램에서 다양한 난수 생성기를 제공하여 임의의 수를 생성하는 데 사용됩니다. 이 라이브러리는 표준 C++11 `<random>` 라이브러리와 유사하지만, 더 많은 기능과 난수 생성기를 포함하고 있습니다.

##### Boost.Random 설치
Boost 라이브러리가 설치되어 있어야 합니다. 설치 방법은 이전 섹션을 참조하세요.

##### 주요 클래스 및 함수

- `boost::random::mt19937`: Mersenne Twister 난수 생성기.
- `boost::random::uniform_int_distribution`: 균등 분포를 따르는 정수 난수 생성기.
- `boost::random::uniform_real_distribution`: 균등 분포를 따르는 실수 난수 생성기.
- `boost::random::normal_distribution`: 정규 분포를 따르는 실수 난수 생성기.

##### 예제 1: 기본적인 난수 생성

```cpp
#include <iostream>
#include <boost/random.hpp>

int main() {
    boost::random::mt19937 gen; // Mersenne Twister 난수 생성기
    boost::random::uniform_int_distribution<> dist(1, 100); // 1부터 100 사이의 정수

    for (int i = 0; i < 10; ++i) {
        std::cout << dist(gen) << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

##### 예제 2: 실수 난수 생성

```cpp
#include <iostream>
#include <boost/random.hpp>

int main() {
    boost::random::mt19937 gen; // Mersenne Twister 난수 생성기
    boost::random::uniform_real_distribution<> dist(0.0, 1.0); // 0.0부터 1.0 사이의 실수

    for (int i = 0; i < 10; ++i) {
        std::cout << dist(gen) << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

##### 예제 3: 정규 분포 난수 생성

```cpp
#include <iostream>
#include <boost/random.hpp>

int main() {
    boost::random::mt19937 gen; // Mersenne Twister 난수 생성기
    boost::random::normal_distribution<> dist(0.0, 1.0); // 평균 0.0, 표준편차 1.0인 정규 분포

    for (int i = 0; i < 10; ++i) {
        std::cout << dist(gen) << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

##### 예제 4: 다양한 분포 사용

Boost.Random 라이브러리는 다양한 분포를 지원합니다. 다음 예제에서는 여러 분포를 사용하는 방법을 보여줍니다.

```cpp
#include <iostream>
#include <boost/random.hpp>

int main() {
    boost::random::mt19937 gen;

    // 균등 분포
    boost::random::uniform_int_distribution<> uniform_dist(1, 10);
    std::cout << "Uniform distribution: ";
    for (int i = 0; i < 5; ++i) {
        std::cout << uniform_dist(gen) << " ";
    }
    std::cout << std::endl;

    // 정규 분포
    boost::random::normal_distribution<> normal_dist(0.0, 1.0);
    std::cout << "Normal distribution: ";
    for (int i = 0; i < 5; ++i) {
        std::cout << normal_dist(gen) << " ";
    }
    std::cout << std::endl;

    // 이산 분포
    boost::random::discrete_distribution<> discrete_dist({30, 10, 60});
    std::cout << "Discrete distribution: ";
    for (int i = 0; i < 5; ++i) {
        std::cout << discrete_dist(gen) << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

##### 예제 5: 시드 설정

난수 생성기의 시드를 설정하여 난수의 예측 가능성을 제어할 수 있습니다. 동일한 시드를 사용하면 동일한 난수 시퀀스를 생성할 수 있습니다.

```cpp
#include <iostream>
#include <boost/random.hpp>

int main() {
    unsigned int seed = 12345;
    boost::random::mt19937 gen(seed); // 시드 설정

    boost::random::uniform_int_distribution<> dist(1, 100);

    std::cout << "Random numbers with seed " << seed << ": ";
    for (int i = 0; i < 10; ++i) {
        std::cout << dist(gen) << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

##### 예제 6: 난수 생성기의 상태 저장 및 복원

Boost.Random은 난수 생성기의 상태를 저장하고 복원할 수 있는 기능을 제공합니다.

```cpp
#include <iostream>
#include <boost/random.hpp>
#include <fstream>

int main() {
    boost::random::mt19937 gen;
    boost::random::uniform_int_distribution<> dist(1, 100);

    std::ofstream ofs("rng_state.dat");
    ofs << gen; // 상태 저장
    ofs.close();

    std::cout << "Random numbers: ";
    for (int i = 0; i < 5; ++i) {
        std::cout << dist(gen) << " ";
    }
    std::cout << std::endl;

    std::ifstream ifs("rng_state.dat");
    ifs >> gen; // 상태 복원
    ifs.close();

    std::cout << "Random numbers after state restoration: ";
    for (int i = 0; i < 5; ++i) {
        std::cout << dist(gen) << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

이 예제에서는 난수 생성기의 상태를 파일에 저장하고, 이를 복원하여 동일한 난수 시퀀스를 생성하는 방법을 보여줍니다.
