### 4. 데이터베이스 라이브러리 (Database Libraries)

#### 4.1. SQLite

SQLite는 경량의 자체 포함 SQL 데이터베이스 엔진으로, 서버가 필요 없는 데이터베이스를 제공합니다. SQLite는 파일 기반 데이터베이스로, 단일 파일에 모든 데이터를 저장합니다.

##### SQLite 설치 및 설정

1. **Windows, macOS, Linux**:
   - [SQLite 다운로드 페이지](https://www.sqlite.org/download.html)에서 SQLite를 다운로드합니다.
   - 프로젝트에 SQLite 헤더 파일(`sqlite3.h`)과 라이브러리 파일(`sqlite3.lib` 또는 `libsqlite3.a`)을 포함합니다.

##### 기본 사용법

SQLite를 사용하여 간단한 데이터베이스를 생성하고 데이터를 삽입 및 조회:

```cpp
#include <sqlite3.h>
#include <iostream>

int main() {
    sqlite3* db;
    char* errMsg = 0;

    int rc = sqlite3_open("test.db", &db);
    if (rc) {
        std::cerr << "Can't open database: " << sqlite3_errmsg(db) << std::endl;
        return rc;
    } else {
        std::cout << "Opened database successfully" << std::endl;
    }

    const char* sqlCreateTable = "CREATE TABLE IF NOT EXISTS COMPANY("
                                 "ID INT PRIMARY KEY NOT NULL,"
                                 "NAME TEXT NOT NULL,"
                                 "AGE INT NOT NULL,"
                                 "ADDRESS CHAR(50),"
                                 "SALARY REAL );";

    rc = sqlite3_exec(db, sqlCreateTable, 0, 0, &errMsg);
    if (rc != SQLITE_OK) {
        std::cerr << "SQL error: " << errMsg << std::endl;
        sqlite3_free(errMsg);
    } else {
        std::cout << "Table created successfully" << std::endl;
    }

    const char* sqlInsert = "INSERT INTO COMPANY (ID, NAME, AGE, ADDRESS, SALARY) "
                            "VALUES (1, 'Paul', 32, 'California', 20000.00 );";
    rc = sqlite3_exec(db, sqlInsert, 0, 0, &errMsg);
    if (rc != SQLITE_OK) {
        std::cerr << "SQL error: " << errMsg << std::endl;
        sqlite3_free(errMsg);
    } else {
        std::cout << "Records created successfully" << std::endl;
    }

    const char* sqlSelect = "SELECT * FROM COMPANY;";
    sqlite3_stmt* stmt;
    rc = sqlite3_prepare_v2(db, sqlSelect, -1, &stmt, 0);
    if (rc != SQLITE_OK) {
        std::cerr << "Failed to fetch data: " << sqlite3_errmsg(db) << std::endl;
    } else {
        while (sqlite3_step(stmt) == SQLITE_ROW) {
            std::cout << "ID: " << sqlite3_column_int(stmt, 0) << std::endl;
            std::cout << "NAME: " << sqlite3_column_text(stmt, 1) << std::endl;
            std::cout << "AGE: " << sqlite3_column_int(stmt, 2) << std::endl;
            std::cout << "ADDRESS: " << sqlite3_column_text(stmt, 3) << std::endl;
            std::cout << "SALARY: " << sqlite3_column_double(stmt, 4) << std::endl;
        }
    }
    sqlite3_finalize(stmt);
    sqlite3_close(db);

    return 0;
}
```

##### 주요 기능

1. **파일 기반 데이터베이스**:
   - SQLite는 모든 데이터를 단일 파일에 저장합니다.
2. **경량 SQL 엔진**:
   - 서버가 필요 없으며, 경량 SQL 엔진으로 쉽게 포함할 수 있습니다.
3. **ACID 준수**:
   - 트랜잭션을 지원하며, ACID(Atomicity, Consistency, Isolation, Durability)를 준수합니다.
4. **크로스 플랫폼**:
   - Windows, macOS, Linux 등 다양한 플랫폼을 지원합니다.
5. **저장 프로시저**:
   - 저장 프로시저를 지원하지 않지만, C API를 사용하여 복잡한 작업을 수행할 수 있습니다.

##### 예제 코드

간단한 데이터베이스를 생성하고 데이터를 삽입 및 조회하는 SQLite 코드입니다:

```cpp
#include <sqlite3.h>
#include <iostream>

int main() {
    sqlite3* db;
    char* errMsg = 0;

    int rc = sqlite3_open("test.db", &db);
    if (rc) {
        std::cerr << "Can't open database: " << sqlite3_errmsg(db) << std::endl;
        return rc;
    } else {
        std::cout << "Opened database successfully" << std::endl;
    }

    const char* sqlCreateTable = "CREATE TABLE IF NOT EXISTS COMPANY("
                                 "ID INT PRIMARY KEY NOT NULL,"
                                 "NAME TEXT NOT NULL,"
                                 "AGE INT NOT NULL,"
                                 "ADDRESS CHAR(50),"
                                 "SALARY REAL );";

    rc = sqlite3_exec(db, sqlCreateTable, 0, 0, &errMsg);
    if (rc != SQLITE_OK) {
        std::cerr << "SQL error: " << errMsg << std::endl;
        sqlite3_free(errMsg);
    } else {
        std::cout << "Table created successfully" << std::endl;
    }

    const char* sqlInsert = "INSERT INTO COMPANY (ID, NAME, AGE, ADDRESS, SALARY) "
                            "VALUES (1, 'Paul', 32, 'California', 20000.00 );";
    rc = sqlite3_exec(db, sqlInsert, 0, 0, &errMsg);
    if (rc != SQLITE_OK) {
        std::cerr << "SQL error: " << errMsg << std::endl;
        sqlite3_free(errMsg);
    } else {
        std::cout << "Records created successfully" << std::endl;
    }

    const char* sqlSelect = "SELECT * FROM COMPANY;";
    sqlite3_stmt* stmt;
    rc = sqlite3_prepare_v2(db, sqlSelect, -1, &stmt, 0);
    if (rc != SQLITE_OK) {
        std::cerr << "Failed to fetch data: " << sqlite3_errmsg(db) << std::endl;
    } else {
        while (sqlite3_step(stmt) == SQLITE_ROW) {
            std::cout << "ID: " << sqlite3_column_int(stmt, 0) << std::endl;
            std::cout << "NAME: " << sqlite3_column_text(stmt, 1) << std::endl;
            std::cout << "AGE: " << sqlite3_column_int(stmt, 2) << std::endl;
            std::cout << "ADDRESS: " << sqlite3_column_text(stmt, 3) << std::endl;
            std::cout << "SALARY: " << sqlite3_column_double(stmt, 4) << std::endl;
        }
    }
    sqlite3_finalize(stmt);
    sqlite3_close(db);

    return 0;
}
```

이 예제에서는 SQLite를 사용하여 데이터베이스를 생성하고, 테이블을 만들고, 데이터를 삽입 및 조회하는 간단한 프로그램을 구현합니다.
