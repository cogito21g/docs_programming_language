#### 2.7. Boost.Serialization

Boost.Serialization 라이브러리는 C++ 객체를 직렬화하고 역직렬화하는 기능을 제공합니다. 이를 통해 객체의 상태를 파일이나 스트림에 저장하고 나중에 복원할 수 있습니다. 직렬화는 데이터를 전송하거나 영구 저장할 때 유용합니다.

##### Boost.Serialization 설치
Boost 라이브러리가 설치되어 있어야 합니다. 설치 방법은 이전 섹션을 참조하세요.

##### 주요 클래스 및 함수

- `boost::archive::text_oarchive`: 객체를 텍스트 형식으로 직렬화하는 아카이브.
- `boost::archive::text_iarchive`: 텍스트 형식으로 저장된 객체를 역직렬화하는 아카이브.
- `boost::serialization::serialize`: 직렬화 및 역직렬화를 지원하는 함수.

##### 예제 1: 기본적인 객체 직렬화 및 역직렬화

```cpp
#include <iostream>
#include <fstream>
#include <boost/archive/text_oarchive.hpp>
#include <boost/archive/text_iarchive.hpp>

class MyClass {
public:
    MyClass() = default;
    MyClass(int a, double b, const std::string& c) : a(a), b(b), c(c) {}

    friend std::ostream& operator<<(std::ostream& os, const MyClass& obj) {
        return os << "a: " << obj.a << ", b: " << obj.b << ", c: " << obj.c;
    }

private:
    int a;
    double b;
    std::string c;

    // Boost.Serialization을 위한 친구 선언
    friend class boost::serialization::access;
    template<class Archive>
    void serialize(Archive& ar, const unsigned int version) {
        ar & a & b & c;
    }
};

int main() {
    MyClass original(1, 2.3, "Boost");

    // 객체 직렬화
    {
        std::ofstream ofs("filename.txt");
        boost::archive::text_oarchive oa(ofs);
        oa << original;
    }

    MyClass loaded;
    // 객체 역직렬화
    {
        std::ifstream ifs("filename.txt");
        boost::archive::text_iarchive ia(ifs);
        ia >> loaded;
    }

    std::cout << "Loaded object: " << loaded << std::endl;

    return 0;
}
```

##### 예제 2: STL 컨테이너 직렬화

Boost.Serialization은 STL 컨테이너의 직렬화도 지원합니다. 다음 예제는 `std::vector`를 직렬화하고 역직렬화하는 방법을 보여줍니다.

```cpp
#include <iostream>
#include <vector>
#include <fstream>
#include <boost/archive/text_oarchive.hpp>
#include <boost/archive/text_iarchive.hpp>

int main() {
    std::vector<int> original = {1, 2, 3, 4, 5};

    // 컨테이너 직렬화
    {
        std::ofstream ofs("vector.txt");
        boost::archive::text_oarchive oa(ofs);
        oa << original;
    }

    std::vector<int> loaded;
    // 컨테이너 역직렬화
    {
        std::ifstream ifs("vector.txt");
        boost::archive::text_iarchive ia(ifs);
        ia >> loaded;
    }

    std::cout << "Loaded vector: ";
    for (int val : loaded) {
        std::cout << val << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

##### 예제 3: 이진 아카이브 사용

Boost.Serialization은 텍스트 아카이브 외에도 이진 아카이브를 지원합니다. 이진 아카이브는 더 작은 크기의 파일을 생성하지만, 인간이 읽을 수 없습니다.

```cpp
#include <iostream>
#include <fstream>
#include <boost/archive/binary_oarchive.hpp>
#include <boost/archive/binary_iarchive.hpp>

class MyClass {
public:
    MyClass() = default;
    MyClass(int a, double b, const std::string& c) : a(a), b(b), c(c) {}

    friend std::ostream& operator<<(std::ostream& os, const MyClass& obj) {
        return os << "a: " << obj.a << ", b: " << obj.b << ", c: " << obj.c;
    }

private:
    int a;
    double b;
    std::string c;

    friend class boost::serialization::access;
    template<class Archive>
    void serialize(Archive& ar, const unsigned int version) {
        ar & a & b & c;
    }
};

int main() {
    MyClass original(1, 2.3, "Boost");

    // 객체 직렬화
    {
        std::ofstream ofs("filename.bin", std::ios::binary);
        boost::archive::binary_oarchive oa(ofs);
        oa << original;
    }

    MyClass loaded;
    // 객체 역직렬화
    {
        std::ifstream ifs("filename.bin", std::ios::binary);
        boost::archive::binary_iarchive ia(ifs);
        ia >> loaded;
    }

    std::cout << "Loaded object: " << loaded << std::endl;

    return 0;
}
```

##### 예제 4: 다형성 클래스 직렬화

Boost.Serialization은 다형성 클래스를 직렬화할 수 있습니다. 이를 위해 클래스 계층 구조에서 각 클래스에 `BOOST_CLASS_EXPORT` 매크로를 사용해야 합니다.

```cpp
#include <iostream>
#include <fstream>
#include <boost/archive/text_oarchive.hpp>
#include <boost/archive/text_iarchive.hpp>
#include <boost/serialization/base_object.hpp>
#include <boost/serialization/export.hpp>

class Base {
public:
    virtual ~Base() = default;
    virtual void print() const = 0;

private:
    friend class boost::serialization::access;
    template<class Archive>
    void serialize(Archive& ar, const unsigned int version) {}
};

class Derived : public Base {
public:
    Derived() = default;
    Derived(int data) : data(data) {}

    void print() const override {
        std::cout << "Derived with data: " << data << std::endl;
    }

private:
    int data;

    friend class boost::serialization::access;
    template<class Archive>
    void serialize(Archive& ar, const unsigned int version) {
        ar & boost::serialization::base_object<Base>(*this);
        ar & data;
    }
};

BOOST_CLASS_EXPORT(Derived)

int main() {
    Base* original = new Derived(42);

    // 객체 직렬화
    {
        std::ofstream ofs("polymorphic.txt");
        boost::archive::text_oarchive oa(ofs);
        oa << original;
    }

    Base* loaded = nullptr;
    // 객체 역직렬화
    {
        std::ifstream ifs("polymorphic.txt");
        boost::archive::text_iarchive ia(ifs);
        ia >> loaded;
    }

    if (loaded) {
        loaded->print();
        delete loaded;
    }

    delete original;
    return 0;
}
```

이 예제에서는 다형성 클래스를 직렬화하고 역직렬화하는 방법을 보여줍니다. `Base` 클래스를 상속하는 `Derived` 클래스를 정의하고, 이를 직렬화합니다.
