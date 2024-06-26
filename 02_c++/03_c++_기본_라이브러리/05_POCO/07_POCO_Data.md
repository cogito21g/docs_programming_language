#### 5.7. POCO Data

POCO Data 라이브러리는 데이터베이스에 접근하고 조작하기 위한 기능을 제공합니다. 이 라이브러리는 SQL 쿼리를 실행하고, 데이터베이스 연결을 관리하며, 다양한 데이터베이스를 지원합니다.

##### 예제 1: SQLite 데이터베이스 연결 및 쿼리 실행

POCO Data 라이브러리를 사용하여 SQLite 데이터베이스에 연결하고 쿼리를 실행하는 예제입니다.

**프로젝트 구조**:
```
SQLiteExample/
├── CMakeLists.txt
└── main.cpp
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.10)

# 프로젝트 이름 설정
project(SQLiteExample)

# C++ 표준 설정
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# POCO 라이브러리 찾기
find_package(Poco REQUIRED Data DataSQLite Foundation)

# 실행 파일 추가
add_executable(SQLiteExample main.cpp)

# POCO 라이브러리 링크
target_link_libraries(SQLiteExample Poco::Data Poco::DataSQLite Poco::Foundation)
```

**main.cpp**:
```cpp
#include <iostream>
#include <Poco/Data/Session.h>
#include <Poco/Data/SQLite/Connector.h>
#include <Poco/Data/SessionFactory.h>

using namespace Poco::Data::Keywords;

int main() {
    // SQLite 커넥터 초기화
    Poco::Data::SQLite::Connector::registerConnector();

    // 세션 생성
    Poco::Data::Session session("SQLite", "sample.db");

    // 테이블 생성
    session << "DROP TABLE IF EXISTS Person", now;
    session << "CREATE TABLE Person (Name VARCHAR(30), Age INTEGER)", now;

    // 데이터 삽입
    session << "INSERT INTO Person VALUES('John Doe', 30)", now;
    session << "INSERT INTO Person VALUES('Jane Doe', 25)", now;

    // 데이터 쿼리
    std::string name;
    int age;
    Poco::Data::Statement select(session);
    select << "SELECT Name, Age FROM Person",
              into(name),
              into(age),
              range(0, 1);

    while (!select.done()) {
        select.execute();
        std::cout << "Name: " << name << ", Age: " << age << std::endl;
    }

    return 0;
}
```

이 예제에서는 `Poco::Data::SQLite::Connector`를 사용하여 SQLite 데이터베이스에 연결하고, `Poco::Data::Session` 클래스를 사용하여 SQL 쿼리를 실행합니다. 테이블을 생성하고 데이터를 삽입한 후, 데이터를 쿼리하여 출력합니다.

##### 예제 2: MySQL 데이터베이스 연결 및 쿼리 실행

POCO Data 라이브러리를 사용하여 MySQL 데이터베이스에 연결하고 쿼리를 실행하는 예제입니다.

**프로젝트 구조**:
```
MySQLExample/
├── CMakeLists.txt
└── main.cpp
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.10)

# 프로젝트 이름 설정
project(MySQLExample)

# C++ 표준 설정
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# POCO 라이브러리 찾기
find_package(Poco REQUIRED Data DataMySQL Foundation)

# 실행 파일 추가
add_executable(MySQLExample main.cpp)

# POCO 라이브러리 링크
target_link_libraries(MySQLExample Poco::Data Poco::DataMySQL Poco::Foundation)
```

**main.cpp**:
```cpp
#include <iostream>
#include <Poco/Data/Session.h>
#include <Poco/Data/MySQL/Connector.h>
#include <Poco/Data/SessionFactory.h>

using namespace Poco::Data::Keywords;

int main() {
    // MySQL 커넥터 초기화
    Poco::Data::MySQL::Connector::registerConnector();

    // 세션 생성
    Poco::Data::Session session("MySQL", "host=localhost;user=root;password=root;db=test;compress=true;auto-reconnect=true");

    // 테이블 생성
    session << "DROP TABLE IF EXISTS Person", now;
    session << "CREATE TABLE Person (Name VARCHAR(30), Age INTEGER)", now;

    // 데이터 삽입
    session << "INSERT INTO Person VALUES('John Doe', 30)", now;
    session << "INSERT INTO Person VALUES('Jane Doe', 25)", now;

    // 데이터 쿼리
    std::string name;
    int age;
    Poco::Data::Statement select(session);
    select << "SELECT Name, Age FROM Person",
              into(name),
              into(age),
              range(0, 1);

    while (!select.done()) {
        select.execute();
        std::cout << "Name: " << name << ", Age: " << age << std::endl;
    }

    return 0;
}
```

이 예제에서는 `Poco::Data::MySQL::Connector`를 사용하여 MySQL 데이터베이스에 연결하고, `Poco::Data::Session` 클래스를 사용하여 SQL 쿼리를 실행합니다. 테이블을 생성하고 데이터를 삽입한 후, 데이터를 쿼리하여 출력합니다.

##### 예제 3: 데이터 바인딩 및 추출

POCO Data 라이브러리는 SQL 쿼리에서 변수를 바인딩하고 추출할 수 있는 기능을 제공합니다.

**프로젝트 구조**:
```
DataBindingExample/
├── CMakeLists.txt
└── main.cpp
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.10)

# 프로젝트 이름 설정
project(DataBindingExample)

# C++ 표준 설정
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# POCO 라이브러리 찾기
find_package(Poco REQUIRED Data DataSQLite Foundation)

# 실행 파일 추가
add_executable(DataBindingExample main.cpp)

# POCO 라이브러리 링크
target_link_libraries(DataBindingExample Poco::Data Poco::DataSQLite Poco::Foundation)
```

**main.cpp**:
```cpp
#include <iostream>
#include <Poco/Data/Session.h>
#include <Poco/Data/SQLite/Connector.h>
#include <Poco/Data/SessionFactory.h>

using namespace Poco::Data::Keywords;

int main() {
    // SQLite 커넥터 초기화
    Poco::Data::SQLite::Connector::registerConnector();

    // 세션 생성
    Poco::Data::Session session("SQLite", "sample.db");

    // 테이블 생성
    session << "DROP TABLE IF EXISTS Person", now;
    session << "CREATE TABLE Person (Name VARCHAR(30), Age INTEGER)", now;

    // 데이터 삽입
    std::string name = "Alice";
    int age = 28;
    session << "INSERT INTO Person VALUES(?, ?)", use(name), use(age), now;

    name = "Bob";
    age = 35;
    session << "INSERT INTO Person VALUES(?, ?)", use(name), use(age), now;

    // 데이터 쿼리
    Poco::Data::Statement select(session);
    select << "SELECT Name, Age FROM Person",
              into(name),
              into(age),
              range(0, 1);

    while (!select.done()) {
        select.execute();
        std::cout << "Name: " << name << ", Age: " << age << std::endl;
    }

    return 0;
}
```

이 예제에서는 `use`와 `into` 키워드를 사용하여 SQL 쿼리에서 변수를 바인딩하고 추출합니다. 변수 `name`과 `age`를 사용하여 데이터를 삽입하고, 데이터를 쿼리하여 출력합니다.
