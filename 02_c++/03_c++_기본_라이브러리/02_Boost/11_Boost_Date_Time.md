#### 2.11. Boost.Date_Time

Boost.Date_Time 라이브러리는 날짜와 시간의 표현 및 계산을 위한 다양한 기능을 제공합니다. 이 라이브러리는 날짜와 시간의 조작, 형식 변환, 시간 차이 계산 등을 쉽게 할 수 있도록 도와줍니다.

##### Boost.Date_Time 설치
Boost 라이브러리가 설치되어 있어야 합니다. 설치 방법은 이전 섹션을 참조하세요.

##### 주요 클래스 및 함수

- `boost::gregorian::date`: 그레고리력 날짜를 나타내는 클래스.
- `boost::posix_time::ptime`: 날짜와 시간을 나타내는 클래스.
- `boost::posix_time::time_duration`: 시간 간격을 나타내는 클래스.
- `boost::posix_time::second_clock`: 현재 시간을 얻기 위한 클래스.
- `boost::posix_time::time_facet`: 날짜와 시간을 문자열로 형식화하기 위한 클래스.

##### 예제 1: 날짜 다루기

```cpp
#include <iostream>
#include <boost/date_time/gregorian/gregorian.hpp>

int main() {
    // 날짜 생성
    boost::gregorian::date d1(2023, 6, 24);
    std::cout << "Date: " << d1 << std::endl;

    // 현재 날짜
    boost::gregorian::date d2 = boost::gregorian::day_clock::local_day();
    std::cout << "Today's date: " << d2 << std::endl;

    // 날짜 계산
    boost::gregorian::date_duration dd(10);
    boost::gregorian::date d3 = d1 + dd;
    std::cout << "Date after 10 days: " << d3 << std::endl;

    // 문자열로부터 날짜 생성
    boost::gregorian::date d4(boost::gregorian::from_simple_string("2023-12-31"));
    std::cout << "Parsed date: " << d4 << std::endl;

    return 0;
}
```

##### 예제 2: 시간 다루기

```cpp
#include <iostream>
#include <boost/date_time/posix_time/posix_time.hpp>

int main() {
    // 시간 생성
    boost::posix_time::ptime t1(boost::gregorian::date(2023, 6, 24), boost::posix_time::hours(12) + boost::posix_time::minutes(30));
    std::cout << "Time: " << t1 << std::endl;

    // 현재 시간
    boost::posix_time::ptime t2 = boost::posix_time::second_clock::local_time();
    std::cout << "Current time: " << t2 << std::endl;

    // 시간 계산
    boost::posix_time::time_duration td = boost::posix_time::hours(2) + boost::posix_time::minutes(45);
    boost::posix_time::ptime t3 = t1 + td;
    std::cout << "Time after 2 hours 45 minutes: " << t3 << std::endl;

    // 문자열로부터 시간 생성
    boost::posix_time::ptime t4(boost::posix_time::time_from_string("2023-06-24 12:30:00"));
    std::cout << "Parsed time: " << t4 << std::endl;

    return 0;
}
```

##### 예제 3: 시간 차이 계산

```cpp
#include <iostream>
#include <boost/date_time/posix_time/posix_time.hpp>

int main() {
    boost::posix_time::ptime t1(boost::gregorian::date(2023, 6, 24), boost::posix_time::hours(12) + boost::posix_time::minutes(30));
    boost::posix_time::ptime t2(boost::gregorian::date(2023, 6, 24), boost::posix_time::hours(15) + boost::posix_time::minutes(45));

    boost::posix_time::time_duration td = t2 - t1;
    std::cout << "Time difference: " << td << std::endl;
    std::cout << "Hours: " << td.hours() << ", Minutes: " << td.minutes() << std::endl;

    return 0;
}
```

##### 예제 4: 날짜와 시간 형식화

```cpp
#include <iostream>
#include <boost/date_time/posix_time/posix_time.hpp>
#include <boost/date_time/date_facet.hpp>

int main() {
    boost::posix_time::ptime t(boost::gregorian::date(2023, 6, 24), boost::posix_time::hours(12) + boost::posix_time::minutes(30));

    // 날짜와 시간을 문자열로 형식화
    std::locale loc(std::cout.getloc(), new boost::posix_time::time_facet("%Y-%m-%d %H:%M:%S"));
    std::cout.imbue(loc);
    std::cout << "Formatted time: " << t << std::endl;

    return 0;
}
```

##### 예제 5: 타임존 처리

Boost.Date_Time은 타임존 처리를 지원합니다.

```cpp
#include <iostream>
#include <boost/date_time/local_time/local_time.hpp>

int main() {
    using namespace boost::local_time;
    using namespace boost::posix_time;
    using namespace boost::gregorian;

    // 타임존 생성
    time_zone_ptr zone(new posix_time_zone("EST-5EDT,M4.1.0,M10.5.0"));
    local_date_time dt1(date(2023, 6, 24), hours(12) + minutes(30), zone, local_date_time::NOT_DATE_TIME_ON_ERROR);

    std::cout << "Local time: " << dt1 << std::endl;
    std::cout << "UTC time: " << dt1.utc_time() << std::endl;

    return 0;
}
```

이 예제에서는 타임존을 생성하고, 특정 타임존의 로컬 시간을 UTC 시간으로 변환하는 방법을 보여줍니다.

##### 예제 6: 날짜와 시간 문자열 파싱

Boost.Date_Time은 다양한 날짜와 시간 형식을 파싱할 수 있습니다.

```cpp
#include <iostream>
#include <boost/date_time/posix_time/posix_time.hpp>

int main() {
    std::string date_str = "2023-06-24 12:30:00";
    boost::posix_time::ptime t(boost::posix_time::time_from_string(date_str));

    std::cout << "Parsed time: " << t << std::endl;

    return 0;
}
```

이 예제에서는 문자열에서 날짜와 시간을 파싱하여 `ptime` 객체로 변환하는 방법을 보여줍니다.
