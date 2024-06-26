#### 4.3. SOCI (The C++ Database Access Library)

SOCI는 C++에서 다양한 데이터베이스에 쉽게 접근할 수 있도록 하는 라이브러리입니다. SOCI는 SQL 구문을 C++ 코드에 직접 삽입하여 데이터베이스와의 상호 작용을 단순화합니다.

##### SOCI 설치 및 설정

1. **Windows**:
   - [SOCI GitHub 페이지](https://github.com/SOCI/soci)에서 SOCI 소스를 다운로드합니다.
   - CMake를 사용하여 SOCI를 빌드하고 설치합니다.
   - Visual Studio 프로젝트에서 SOCI의 include 디렉토리와 lib 디렉토리를 설정합니다.

2. **macOS**:
   - Homebrew를 사용하여 SOCI를 설치합니다:
     ```bash
     brew install soci
     ```

3. **Linux**:
   - 패키지 관리자를 사용하여 SOCI를 설치합니다:
     ```bash
     sudo apt-get install libsqlite3-dev libpq-dev
     sudo apt-get install libmysqlclient-dev
     sudo apt-get install libsoci-dev
     ```

##### 기본 사용법

SOCI를 사용하여 SQLite 데이터베이스에 연결하고 데이터를 삽입 및 조회:

```cpp
#include <soci/soci.h>
#include <soci/sqlite3/soci-sqlite3.h>
#include <iostream>

int main() {
    try {
        soci::session sql(soci::sqlite3, "test.db");

        sql << "CREATE TABLE IF NOT EXISTS COMPANY("
               "ID INT PRIMARY KEY NOT NULL,"
               "NAME TEXT NOT NULL,"
               "AGE INT NOT NULL,"
               "ADDRESS CHAR(50),"
               "SALARY REAL );";

        int id = 1;
        std::string name = "Paul";
        int age = 32;
        std::string address = "California";
        double salary = 20000.00;

        sql << "INSERT INTO COMPANY (ID, NAME, AGE, ADDRESS, SALARY) VALUES(:id, :name, :age, :address, :salary)",
            soci::use(id), soci::use(name), soci::use(age), soci::use(address), soci::use(salary);

        soci::rowset<soci::row> rs = (sql.prepare << "SELECT * FROM COMPANY");

        for (auto it = rs.begin(); it != rs.end(); ++it) {
            const soci::row& row = *it;
            std::cout << "ID: " << row.get<int>(0) << std::endl;
            std::cout << "NAME: " << row.get<std::string>(1) << std::endl;
            std::cout << "AGE: " << row.get<int>(2) << std::endl;
            std::cout << "ADDRESS: " << row.get<std::string>(3) << std::endl;
            std::cout << "SALARY: " << row.get<double>(4) << std::endl;
        }
    } catch (soci::soci_error const &e) {
        std::cerr << "Error: " << e.what() << std::endl;
    }

    return 0;
}
```

##### 주요 기능

1. **다양한 데이터베이스 지원**:
   - SOCI는 SQLite, MySQL, PostgreSQL, Oracle, Firebird 등의 다양한 데이터베이스를 지원합니다.
2. **SQL 구문 내장**:
   - SQL 구문을 C++ 코드에 직접 삽입하여 데이터베이스와의 상호 작용을 단순화합니다.
3. **프리페어드 스테이트먼트**:
   - 프리페어드 스테이트먼트를 사용하여 효율적이고 안전하게 쿼리를 실행할 수 있습니다.
4. **트랜잭션 관리**:
   - 트랜잭션을 시작, 커밋, 롤백할 수 있습니다.
5. **유연한 데이터 매핑**:
   - 데이터베이스의 결과를 C++의 다양한 데이터 타입으로 매핑할 수 있습니다.

##### 예제 코드

간단한 데이터베이스에 연결하고 데이터를 삽입 및 조회하는 SOCI 코드입니다:

```cpp
#include <soci/soci.h>
#include <soci/sqlite3/soci-sqlite3.h>
#include <iostream>

int main() {
    try {
        soci::session sql(soci::sqlite3, "test.db");

        sql << "CREATE TABLE IF NOT EXISTS COMPANY("
               "ID INT PRIMARY KEY NOT NULL,"
               "NAME TEXT NOT NULL,"
               "AGE INT NOT NULL,"
               "ADDRESS CHAR(50),"
               "SALARY REAL );";

        int id = 1;
        std::string name = "Paul";
        int age = 32;
        std::string address = "California";
        double salary = 20000.00;

        sql << "INSERT INTO COMPANY (ID, NAME, AGE, ADDRESS, SALARY) VALUES(:id, :name, :age, :address, :salary)",
            soci::use(id), soci::use(name), soci::use(age), soci::use(address), soci::use(salary);

        soci::rowset<soci::row> rs = (sql.prepare << "SELECT * FROM COMPANY");

        for (auto it = rs.begin(); it != rs.end(); ++it) {
            const soci::row& row = *it;
            std::cout << "ID: " << row.get<int>(0) << std::endl;
            std::cout << "NAME: " << row.get<std::string>(1) << std::endl;
            std::cout << "AGE: " << row.get<int>(2) << std::endl;
            std::cout << "ADDRESS: " << row.get<std::string>(3) << std::endl;
            std::cout << "SALARY: " << row.get<double>(4) << std::endl;
        }
    } catch (soci::soci_error const &e) {
        std::cerr << "Error: " << e.what() << std::endl;
    }

    return 0;
}
```

이 예제에서는 SOCI를 사용하여 데이터베이스에 연결하고, 테이블을 만들고, 데이터를 삽입 및 조회하는 간단한 프로그램을 구현합니다.
