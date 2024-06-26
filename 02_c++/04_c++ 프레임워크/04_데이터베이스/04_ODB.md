#### 4.4. ODB (C++ Object-Relational Mapping)

ODB는 C++용 객체-관계 매핑(ORM) 라이브러리로, 객체 지향 방식으로 데이터베이스와 상호작용할 수 있게 해줍니다. ODB는 SQL 쿼리를 자동으로 생성하여 객체를 데이터베이스에 저장하고, 불러오는 작업을 단순화합니다.

##### ODB 설치 및 설정

1. **Windows, macOS, Linux**:
   - [ODB 다운로드 페이지](https://codesynthesis.com/products/odb/download.xhtml)에서 ODB를 다운로드합니다.
   - ODB 컴파일러와 라이브러리를 설치합니다.
   - 프로젝트에서 ODB의 include 디렉토리와 lib 디렉토리를 설정합니다.
   - 필요한 라이브러리를 링크합니다: `libodb`, `libodb-sqlite`, `libodb-mysql` 등.

##### 기본 사용법

ODB를 사용하여 SQLite 데이터베이스에 객체를 저장하고 조회:

1. **클래스 정의 및 매핑**

```cpp
#include <odb/core.hxx>

#pragma db object
class Person {
public:
    Person(const std::string& name, int age)
        : name_(name), age_(age) {}

    const std::string& name() const { return name_; }
    int age() const { return age_; }

private:
    friend class odb::access;

    Person() {}

    #pragma db id auto
    int id_;

    std::string name_;
    int age_;
};
```

2. **데이터베이스 작업**

```cpp
#include <odb/database.hxx>
#include <odb/sqlite/database.hxx>
#include <odb/transaction.hxx>
#include <memory>
#include <iostream>

// Include generated code
#include "person-odb.hxx"

int main() {
    try {
        std::shared_ptr<odb::database> db(new odb::sqlite::database("test.db"));

        {
            // Create the database schema
            odb::transaction t(db->begin());
            db->schema_catalog().create_schema();
            t.commit();
        }

        {
            // Insert a new person
            Person john("John Doe", 30);
            odb::transaction t(db->begin());
            db->persist(john);
            t.commit();
        }

        {
            // Query the database
            odb::transaction t(db->begin());
            odb::result<Person> r(db->query<Person>());

            for (odb::result<Person>::iterator i(r.begin()); i != r.end(); ++i) {
                std::cout << "ID: " << i->id() << ", Name: " << i->name() << ", Age: " << i->age() << std::endl;
            }
            t.commit();
        }
    } catch (const odb::exception& e) {
        std::cerr << e.what() << std::endl;
        return 1;
    }

    return 0;
}
```

##### 주요 기능

1. **객체-관계 매핑**:
   - C++ 객체를 데이터베이스 테이블에 매핑하여 SQL 쿼리를 자동으로 생성합니다.
2. **다양한 데이터베이스 지원**:
   - SQLite, MySQL, PostgreSQL, Oracle, MS SQL 등 다양한 데이터베이스를 지원합니다.
3. **트랜잭션 관리**:
   - 트랜잭션을 시작, 커밋, 롤백할 수 있습니다.
4. **프리페어드 쿼리**:
   - 프리페어드 쿼리를 사용하여 효율적이고 안전하게 데이터베이스 작업을 수행할 수 있습니다.
5. **데이터베이스 스키마 관리**:
   - 데이터베이스 스키마를 자동으로 생성하고, 업데이트할 수 있습니다.

##### 예제 코드

간단한 데이터베이스에 객체를 저장하고 조회하는 ODB 코드입니다:

1. **클래스 정의 및 매핑**

```cpp
#include <odb/core.hxx>

#pragma db object
class Person {
public:
    Person(const std::string& name, int age)
        : name_(name), age_(age) {}

    const std::string& name() const { return name_; }
    int age() const { return age_; }

private:
    friend class odb::access;

    Person() {}

    #pragma db id auto
    int id_;

    std::string name_;
    int age_;
};
```

2. **데이터베이스 작업**

```cpp
#include <odb/database.hxx>
#include <odb/sqlite/database.hxx>
#include <odb/transaction.hxx>
#include <memory>
#include <iostream>

// Include generated code
#include "person-odb.hxx"

int main() {
    try {
        std::shared_ptr<odb::database> db(new odb::sqlite::database("test.db"));

        {
            // Create the database schema
            odb::transaction t(db->begin());
            db->schema_catalog().create_schema();
            t.commit();
        }

        {
            // Insert a new person
            Person john("John Doe", 30);
            odb::transaction t(db->begin());
            db->persist(john);
            t.commit();
        }

        {
            // Query the database
            odb::transaction t(db->begin());
            odb::result<Person> r(db->query<Person>());

            for (odb::result<Person>::iterator i(r.begin()); i != r.end(); ++i) {
                std::cout << "ID: " << i->id() << ", Name: " << i->name() << ", Age: " << i->age() << std::endl;
            }
            t.commit();
        }
    } catch (const odb::exception& e) {
        std::cerr << e.what() << std::endl;
        return 1;
    }

    return 0;
}
```

이 예제에서는 ODB를 사용하여 데이터베이스에 객체를 저장하고, 데이터를 조회하는 간단한 프로그램을 구현합니다. 클래스 정의 및 매핑, 데이터베이스 작업을 포함합니다.
