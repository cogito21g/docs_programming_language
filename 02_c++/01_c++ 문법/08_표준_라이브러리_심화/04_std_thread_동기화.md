#### 8.4. std::thread와 동기화 (mutex, lock 등)

멀티스레딩은 여러 스레드를 사용하여 병렬로 작업을 수행함으로써 성능을 향상시키는 프로그래밍 기법입니다. C++11부터 표준 라이브러리에서 스레드와 동기화 기능을 제공합니다. 이 기능에는 `std::thread`, `std::mutex`, `std::lock_guard`, `std::unique_lock` 등이 포함됩니다.

##### std::thread

`std::thread`는 C++에서 스레드를 생성하고 관리하기 위한 클래스입니다. 스레드를 생성할 때 실행할 함수를 인자로 전달합니다.

###### 예제

```cpp
#include <iostream>
#include <thread>
using namespace std;

void printMessage(const string& message) {
    cout << message << endl;
}

int main() {
    thread t1(printMessage, "Hello from thread 1");
    thread t2(printMessage, "Hello from thread 2");

    t1.join(); // 스레드가 완료될 때까지 대기
    t2.join(); // 스레드가 완료될 때까지 대기

    return 0;
}
```

이 예제에서 `std::thread`를 사용하여 두 개의 스레드를 생성하고, 각 스레드에서 메시지를 출력합니다. `join` 메서드는 스레드가 완료될 때까지 메인 스레드가 대기하도록 합니다.

##### std::mutex와 std::lock_guard

`std::mutex`는 여러 스레드가 공유 리소스에 동시 접근하는 것을 방지하는 상호 배제 락(mutex)을 제공합니다. `std::lock_guard`는 스코프 기반으로 mutex를 잠그고 자동으로 해제하는 RAII 스타일의 락입니다.

###### 예제

```cpp
#include <iostream>
#include <thread>
#include <mutex>
using namespace std;

mutex mtx;

void printMessage(const string& message) {
    lock_guard<mutex> lock(mtx);
    cout << message << endl;
}

int main() {
    thread t1(printMessage, "Hello from thread 1");
    thread t2(printMessage, "Hello from thread 2");

    t1.join();
    t2.join();

    return 0;
}
```

이 예제에서 `std::mutex`와 `std::lock_guard`를 사용하여 두 스레드가 안전하게 메시지를 출력하도록 합니다. `lock_guard`는 스코프가 끝날 때 자동으로 mutex를 해제합니다.

##### std::unique_lock

`std::unique_lock`은 `std::lock_guard`보다 유연한 락으로, 수동으로 잠그고 해제할 수 있으며, 조건 변수와 함께 사용할 수 있습니다.

###### 예제

```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <condition_variable>
using namespace std;

mutex mtx;
condition_variable cv;
bool ready = false;

void printMessage(const string& message) {
    unique_lock<mutex> lock(mtx);
    cv.wait(lock, [] { return ready; }); // ready가 true가 될 때까지 대기
    cout << message << endl;
}

int main() {
    thread t1(printMessage, "Hello from thread 1");
    thread t2(printMessage, "Hello from thread 2");

    this_thread::sleep_for(chrono::seconds(1));

    {
        lock_guard<mutex> lock(mtx);
        ready = true;
    }
    cv.notify_all(); // 모든 대기 중인 스레드에게 알림

    t1.join();
    t2.join();

    return 0;
}
```

이 예제에서 `std::unique_lock`과 `std::condition_variable`을 사용하여 스레드가 특정 조건이 충족될 때까지 대기하도록 합니다. `notify_all` 메서드는 모든 대기 중인 스레드에게 조건이 충족되었음을 알립니다.

##### std::atomic

`std::atomic`은 원자 연산을 제공하여, 동기화 없이도 여러 스레드가 안전하게 변수를 읽고 쓸 수 있도록 합니다.

###### 예제

```cpp
#include <iostream>
#include <thread>
#include <atomic>
using namespace std;

atomic<int> counter(0);

void increment() {
    for (int i = 0; i < 1000; ++i) {
        ++counter; // 원자적 증가
    }
}

int main() {
    thread t1(increment);
    thread t2(increment);

    t1.join();
    t2.join();

    cout << "Final counter value: " << counter << endl;

    return 0;
}
```

이 예제에서 `std::atomic`을 사용하여 여러 스레드가 안전하게 공유 변수를 증가시킵니다.

#### 요약

- **std::thread**: 스레드를 생성하고 관리하는 클래스입니다.
- **std::mutex**: 상호 배제 락을 제공하여 여러 스레드가 공유 리소스에 동시 접근하는 것을 방지합니다.
- **std::lock_guard**: 스코프 기반으로 mutex를 잠그고 자동으로 해제하는 RAII 스타일의 락입니다.
- **std::unique_lock**: 수동으로 잠그고 해제할 수 있는 유연한 락으로, 조건 변수와 함께 사용할 수 있습니다.
- **std::condition_variable**: 스레드가 특정 조건이 충족될 때까지 대기하도록 합니다.
- **std::atomic**: 원자 연산을 제공하여, 동기화 없이도 여러 스레드가 안전하게 변수를 읽고 쓸 수 있도록 합니다.
