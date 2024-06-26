#### 2.5. Boost.Thread

Boost.Thread 라이브러리는 멀티스레딩을 쉽게 구현할 수 있는 다양한 기능을 제공합니다. 스레드 생성, 동기화 메커니즘 (뮤텍스, 조건 변수 등), 잠금 관리 등을 포함하여 C++11의 스레딩 기능과 유사한 인터페이스를 제공합니다.

##### Boost.Thread 설치
Boost 라이브러리가 설치되어 있어야 합니다. 설치 방법은 이전 섹션을 참조하세요.

##### 주요 클래스 및 함수

- `boost::thread`: 스레드를 생성하고 제어하는 클래스.
- `boost::mutex`: 상호 배제를 위한 뮤텍스 클래스.
- `boost::unique_lock`: 뮤텍스를 안전하게 잠그고 해제하는 클래스.
- `boost::condition_variable`: 스레드 간의 통신을 위한 조건 변수 클래스.

##### 예제 1: 기본적인 스레드 생성

```cpp
#include <iostream>
#include <boost/thread.hpp>

void printHello() {
    std::cout << "Hello from thread!" << std::endl;
}

int main() {
    boost::thread t(printHello);

    t.join(); // 스레드가 종료될 때까지 대기

    std::cout << "Thread has finished execution" << std::endl;

    return 0;
}
```

##### 예제 2: 뮤텍스와 unique_lock

```cpp
#include <iostream>
#include <boost/thread.hpp>
#include <boost/chrono.hpp>

boost::mutex mtx;

void printNumber(int n) {
    boost::unique_lock<boost::mutex> lock(mtx);
    std::cout << "Thread " << n << std::endl;
    boost::this_thread::sleep_for(boost::chrono::seconds(1));
}

int main() {
    boost::thread t1(printNumber, 1);
    boost::thread t2(printNumber, 2);

    t1.join();
    t2.join();

    return 0;
}
```

##### 예제 3: 조건 변수

```cpp
#include <iostream>
#include <boost/thread.hpp>
#include <boost/chrono.hpp>

boost::mutex mtx;
boost::condition_variable cv;
bool ready = false;

void printID(int id) {
    boost::unique_lock<boost::mutex> lock(mtx);
    while (!ready) {
        cv.wait(lock);
    }
    std::cout << "Thread " << id << std::endl;
}

void setReady() {
    boost::this_thread::sleep_for(boost::chrono::seconds(1));
    {
        boost::unique_lock<boost::mutex> lock(mtx);
        ready = true;
    }
    cv.notify_all();
}

int main() {
    boost::thread threads[10];
    for (int i = 0; i < 10; ++i) {
        threads[i] = boost::thread(printID, i);
    }

    boost::thread(setReady).detach();

    for (auto& th : threads) {
        th.join();
    }

    return 0;
}
```

##### 예제 4: 스레드 간 데이터 공유

```cpp
#include <iostream>
#include <vector>
#include <boost/thread.hpp>
#include <boost/chrono.hpp>

boost::mutex mtx;
std::vector<int> sharedData;

void producer() {
    for (int i = 0; i < 5; ++i) {
        boost::unique_lock<boost::mutex> lock(mtx);
        sharedData.push_back(i);
        std::cout << "Produced: " << i << std::endl;
        boost::this_thread::sleep_for(boost::chrono::seconds(1));
    }
}

void consumer() {
    for (int i = 0; i < 5; ++i) {
        boost::unique_lock<boost::mutex> lock(mtx);
        if (!sharedData.empty()) {
            int data = sharedData.back();
            sharedData.pop_back();
            std::cout << "Consumed: " << data << std::endl;
        }
        boost::this_thread::sleep_for(boost::chrono::milliseconds(500));
    }
}

int main() {
    boost::thread prodThread(producer);
    boost::thread consThread(consumer);

    prodThread.join();
    consThread.join();

    return 0;
}
```

##### 예제 5: 스레드 풀 구현

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <boost/thread.hpp>

class ThreadPool {
public:
    ThreadPool(size_t numThreads);
    ~ThreadPool();

    void enqueue(std::function<void()> task);

private:
    std::vector<boost::thread> workers;
    std::queue<std::function<void()>> tasks;

    boost::mutex queueMutex;
    boost::condition_variable condition;
    bool stop;

    void workerThread();
};

ThreadPool::ThreadPool(size_t numThreads) : stop(false) {
    for (size_t i = 0; i < numThreads; ++i) {
        workers.emplace_back(boost::bind(&ThreadPool::workerThread, this));
    }
}

ThreadPool::~ThreadPool() {
    {
        boost::unique_lock<boost::mutex> lock(queueMutex);
        stop = true;
    }
    condition.notify_all();
    for (auto& worker : workers) {
        worker.join();
    }
}

void ThreadPool::enqueue(std::function<void()> task) {
    {
        boost::unique_lock<boost::mutex> lock(queueMutex);
        tasks.push(task);
    }
    condition.notify_one();
}

void ThreadPool::workerThread() {
    while (true) {
        std::function<void()> task;
        {
            boost::unique_lock<boost::mutex> lock(queueMutex);
            condition.wait(lock, [this] { return stop || !tasks.empty(); });
            if (stop && tasks.empty()) {
                return;
            }
            task = tasks.front();
            tasks.pop();
        }
        task();
    }
}

int main() {
    ThreadPool pool(4);

    for (int i = 0; i < 10; ++i) {
        pool.enqueue([i] {
            std::cout << "Task " << i << " is being processed by thread " << boost::this_thread::get_id() << std::endl;
            boost::this_thread::sleep_for(boost::chrono::seconds(1));
        });
    }

    boost::this_thread::sleep_for(boost::chrono::seconds(5));

    return 0;
}
```

이 예제에서는 간단한 스레드 풀을 구현합니다. 스레드 풀은 여러 작업을 병렬로 처리하여 성능을 향상시킬 수 있습니다.
