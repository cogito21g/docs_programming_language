#### 4.2. MySQL Connector/C++

MySQL Connector/C++는 C++ 애플리케이션에서 MySQL 데이터베이스에 연결하고 작업할 수 있게 해주는 라이브러리입니다. SQL 쿼리를 실행하고, 결과를 가져오고, 트랜잭션을 관리하는 등의 기능을 제공합니다.

##### MySQL Connector/C++ 설치 및 설정

1. **Windows, macOS, Linux**:
   - [MySQL Connector/C++ 다운로드 페이지](https://dev.mysql.com/downloads/connector/cpp/)에서 MySQL Connector/C++를 다운로드합니다.
   - 압축을 풀고, MySQL Connector/C++의 include 디렉토리와 lib 디렉토리를 프로젝트에 추가합니다.
   - 필요한 라이브러리를 링크합니다: `mysqlcppconn8.lib` (또는 `libmysqlcppconn8.a`).

##### 기본 사용법

MySQL Connector/C++를 사용하여 데이터베이스에 연결하고 데이터를 삽입 및 조회:

```cpp
#include <mysql_driver.h>
#include <mysql_connection.h>
#include <cppconn/statement.h>
#include <cppconn/resultset.h>
#include <cppconn/prepared_statement.h>
#include <iostream>

int main() {
    try {
        sql::mysql::MySQL_Driver* driver = sql::mysql::get_mysql_driver_instance();
        std::unique_ptr<sql::Connection> con(driver->connect("tcp://127.0.0.1:3306", "user", "password"));
        
        con->setSchema("testdb");

        std::unique_ptr<sql::Statement> stmt(con->createStatement());
        stmt->execute("CREATE TABLE IF NOT EXISTS COMPANY(ID INT PRIMARY KEY, NAME VARCHAR(255), AGE INT, ADDRESS VARCHAR(255), SALARY DOUBLE)");

        std::unique_ptr<sql::PreparedStatement> pstmt(con->prepareStatement("INSERT INTO COMPANY(ID, NAME, AGE, ADDRESS, SALARY) VALUES (?, ?, ?, ?, ?)"));
        pstmt->setInt(1, 1);
        pstmt->setString(2, "Paul");
        pstmt->setInt(3, 32);
        pstmt->setString(4, "California");
        pstmt->setDouble(5, 20000.00);
        pstmt->execute();

        std::unique_ptr<sql::ResultSet> res(stmt->executeQuery("SELECT * FROM COMPANY"));

        while (res->next()) {
            std::cout << "ID: " << res->getInt("ID") << std::endl;
            std::cout << "NAME: " << res->getString("NAME") << std::endl;
            std::cout << "AGE: " << res->getInt("AGE") << std::endl;
            std::cout << "ADDRESS: " << res->getString("ADDRESS") << std::endl;
            std::cout << "SALARY: " << res->getDouble("SALARY") << std::endl;
        }
    } catch (sql::SQLException& e) {
        std::cerr << "SQLException: " << e.what() << std::endl;
        std::cerr << "SQLState: " << e.getSQLState() << std::endl;
    }

    return 0;
}
```

##### 주요 기능

1. **데이터베이스 연결**:
   - MySQL 데이터베이스에 연결하고, 연결을 관리할 수 있습니다.
2. **SQL 쿼리 실행**:
   - SQL 쿼리(SELECT, INSERT, UPDATE, DELETE 등)를 실행할 수 있습니다.
3. **트랜잭션 관리**:
   - 트랜잭션을 시작, 커밋, 롤백할 수 있습니다.
4. **프리페어드 스테이트먼트**:
   - 프리페어드 스테이트먼트를 사용하여 효율적이고 안전하게 쿼리를 실행할 수 있습니다.
5. **결과 집합 처리**:
   - SQL 쿼리의 결과를 처리하고, 각 행과 열에 접근할 수 있습니다.

##### 예제 코드

간단한 데이터베이스에 연결하고 데이터를 삽입 및 조회하는 MySQL Connector/C++ 코드입니다:

```cpp
#include <mysql_driver.h>
#include <mysql_connection.h>
#include <cppconn/statement.h>
#include <cppconn/resultset.h>
#include <cppconn/prepared_statement.h>
#include <iostream>

int main() {
    try {
        sql::mysql::MySQL_Driver* driver = sql::mysql::get_mysql_driver_instance();
        std::unique_ptr<sql::Connection> con(driver->connect("tcp://127.0.0.1:3306", "user", "password"));
        
        con->setSchema("testdb");

        std::unique_ptr<sql::Statement> stmt(con->createStatement());
        stmt->execute("CREATE TABLE IF NOT EXISTS COMPANY(ID INT PRIMARY KEY, NAME VARCHAR(255), AGE INT, ADDRESS VARCHAR(255), SALARY DOUBLE)");

        std::unique_ptr<sql::PreparedStatement> pstmt(con->prepareStatement("INSERT INTO COMPANY(ID, NAME, AGE, ADDRESS, SALARY) VALUES (?, ?, ?, ?, ?)"));
        pstmt->setInt(1, 1);
        pstmt->setString(2, "Paul");
        pstmt->setInt(3, 32);
        pstmt->setString(4, "California");
        pstmt->setDouble(5, 20000.00);
        pstmt->execute();

        std::unique_ptr<sql::ResultSet> res(stmt->executeQuery("SELECT * FROM COMPANY"));

        while (res->next()) {
            std::cout << "ID: " << res->getInt("ID") << std::endl;
            std::cout << "NAME: " << res->getString("NAME") << std::endl;
            std::cout << "AGE: " << res->getInt("AGE") << std::endl;
            std::cout << "ADDRESS: " << res->getString("ADDRESS") << std::endl;
            std::cout << "SALARY: " << res->getDouble("SALARY") << std::endl;
        }
    } catch (sql::SQLException& e) {
        std::cerr << "SQLException: " << e.what() << std::endl;
        std::cerr << "SQLState: " << e.getSQLState() << std::endl;
    }

    return 0;
}
```

이 예제에서는 MySQL Connector/C++를 사용하여 데이터베이스에 연결하고, 테이블을 만들고, 데이터를 삽입 및 조회하는 간단한 프로그램을 구현합니다.
