#### 2.3. Boost.Filesystem

Boost.Filesystem 라이브러리는 파일 시스템 조작을 위한 클래스와 함수를 제공합니다. 이를 통해 디렉터리 탐색, 파일 정보 가져오기, 파일 및 디렉터리 생성/삭제 등의 작업을 수행할 수 있습니다.

##### Boost.Filesystem 설치
Boost 라이브러리가 설치되어 있어야 합니다. 설치 방법은 이전 섹션을 참조하세요.

##### 주요 클래스 및 함수

- `boost::filesystem::path`: 파일 또는 디렉터리 경로를 나타내는 클래스.
- `boost::filesystem::exists`: 경로가 존재하는지 확인하는 함수.
- `boost::filesystem::is_directory`: 경로가 디렉터리인지 확인하는 함수.
- `boost::filesystem::create_directory`: 디렉터리를 생성하는 함수.
- `boost::filesystem::remove`: 파일 또는 디렉터리를 삭제하는 함수.
- `boost::filesystem::directory_iterator`: 디렉터리의 내용을 반복하는 반복자.

##### 예제 1: 파일 및 디렉터리 생성 및 삭제

```cpp
#include <iostream>
#include <boost/filesystem.hpp>

namespace fs = boost::filesystem;

int main() {
    fs::path dir("example_dir");

    // 디렉터리 생성
    if (fs::create_directory(dir)) {
        std::cout << "Directory created: " << dir << std::endl;
    }

    // 파일 생성
    fs::path file = dir / "example_file.txt";
    std::ofstream(file.c_str()) << "Hello, Boost.Filesystem!" << std::endl;
    std::cout << "File created: " << file << std::endl;

    // 파일 삭제
    if (fs::remove(file)) {
        std::cout << "File removed: " << file << std::endl;
    }

    // 디렉터리 삭제
    if (fs::remove(dir)) {
        std::cout << "Directory removed: " << dir << std::endl;
    }

    return 0;
}
```

##### 예제 2: 디렉터리 내용 나열

```cpp
#include <iostream>
#include <boost/filesystem.hpp>

namespace fs = boost::filesystem;

int main() {
    fs::path dir(".");

    if (fs::exists(dir) && fs::is_directory(dir)) {
        std::cout << "Contents of " << dir << ":\n";
        for (const auto& entry : fs::directory_iterator(dir)) {
            std::cout << entry.path().string() << std::endl;
        }
    } else {
        std::cerr << "Directory does not exist: " << dir << std::endl;
    }

    return 0;
}
```

##### 예제 3: 파일 정보 가져오기

```cpp
#include <iostream>
#include <boost/filesystem.hpp>

namespace fs = boost::filesystem;

int main() {
    fs::path file("example.txt");

    if (fs::exists(file)) {
        std::cout << "File size: " << fs::file_size(file) << " bytes" << std::endl;
        std::cout << "Last modified: " << fs::last_write_time(file) << std::endl;
    } else {
        std::cerr << "File does not exist: " << file << std::endl;
    }

    return 0;
}
```

##### 예제 4: 디렉터리 복사

```cpp
#include <iostream>
#include <boost/filesystem.hpp>

namespace fs = boost::filesystem;

void copy_directory(const fs::path& source, const fs::path& destination) {
    if (!fs::exists(source) || !fs::is_directory(source)) {
        throw std::runtime_error("Source directory does not exist or is not a directory");
    }

    if (fs::exists(destination)) {
        throw std::runtime_error("Destination directory already exists");
    }

    if (!fs::create_directory(destination)) {
        throw std::runtime_error("Unable to create destination directory");
    }

    for (const auto& entry : fs::directory_iterator(source)) {
        const auto& path = entry.path();
        auto relative_path_str = path.string().substr(source.string().length());
        fs::copy(path, destination / relative_path_str);
    }
}

int main() {
    try {
        fs::path src("example_dir");
        fs::path dst("example_dir_copy");

        copy_directory(src, dst);
        std::cout << "Directory copied from " << src << " to " << dst << std::endl;
    } catch (const std::exception& ex) {
        std::cerr << "Error: " << ex.what() << std::endl;
    }

    return 0;
}
```

이 예제에서는 디렉터리를 복사하는 함수를 구현합니다. `copy_directory` 함수는 소스 디렉터리의 내용을 반복하여 대상 디렉터리에 복사합니다.
