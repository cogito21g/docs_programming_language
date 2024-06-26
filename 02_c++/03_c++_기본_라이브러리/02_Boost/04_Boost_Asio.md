#### 2.4. Boost.Asio

Boost.Asio는 네트워킹 및 저수준 I/O 프로그래밍을 위한 라이브러리입니다. 이는 비동기 방식의 네트워크 프로그래밍을 지원하며, TCP, UDP, HTTP 등 다양한 프로토콜을 사용할 수 있습니다.

##### Boost.Asio 설치
Boost 라이브러리가 설치되어 있어야 합니다. 설치 방법은 이전 섹션을 참조하세요.

##### 주요 클래스 및 함수

- `boost::asio::io_context`: 모든 비동기 I/O 작업의 기본 클래스입니다.
- `boost::asio::ip::tcp::socket`: TCP 소켓 클래스입니다.
- `boost::asio::ip::tcp::acceptor`: TCP 연결 수락기 클래스입니다.
- `boost::asio::ip::udp::socket`: UDP 소켓 클래스입니다.
- `boost::asio::steady_timer`: 타이머 클래스입니다.

##### 예제 1: 기본적인 TCP 서버

```cpp
#include <iostream>
#include <boost/asio.hpp>

using boost::asio::ip::tcp;

void handle_session(tcp::socket sock) {
    try {
        for (;;) {
            char data[1024];
            boost::system::error_code error;
            size_t length = sock.read_some(boost::asio::buffer(data), error);

            if (error == boost::asio::error::eof) {
                break; // 연결이 닫혔습니다.
            } else if (error) {
                throw boost::system::system_error(error);
            }

            boost::asio::write(sock, boost::asio::buffer(data, length));
        }
    } catch (std::exception& e) {
        std::cerr << "Exception in thread: " << e.what() << "\n";
    }
}

int main() {
    try {
        boost::asio::io_context io_context;

        tcp::acceptor acceptor(io_context, tcp::endpoint(tcp::v4(), 12345));

        for (;;) {
            tcp::socket socket(io_context);
            acceptor.accept(socket);
            std::thread(handle_session, std::move(socket)).detach();
        }
    } catch (std::exception& e) {
        std::cerr << "Exception: " << e.what() << "\n";
    }

    return 0;
}
```

##### 예제 2: 기본적인 TCP 클라이언트

```cpp
#include <iostream>
#include <boost/asio.hpp>

using boost::asio::ip::tcp;

int main() {
    try {
        boost::asio::io_context io_context;

        tcp::resolver resolver(io_context);
        tcp::resolver::results_type endpoints = resolver.resolve("127.0.0.1", "12345");

        tcp::socket socket(io_context);
        boost::asio::connect(socket, endpoints);

        std::string message = "Hello from client";
        boost::asio::write(socket, boost::asio::buffer(message));

        char reply[1024];
        size_t reply_length = boost::asio::read(socket, boost::asio::buffer(reply, message.size()));
        std::cout << "Reply from server: ";
        std::cout.write(reply, reply_length);
        std::cout << "\n";
    } catch (std::exception& e) {
        std::cerr << "Exception: " << e.what() << "\n";
    }

    return 0;
}
```

##### 예제 3: 비동기 TCP 서버

```cpp
#include <iostream>
#include <boost/asio.hpp>

using boost::asio::ip::tcp;

class Server {
public:
    Server(boost::asio::io_context& io_context, short port)
        : acceptor_(io_context, tcp::endpoint(tcp::v4(), port)) {
        do_accept();
    }

private:
    void do_accept() {
        acceptor_.async_accept(
            [this](boost::system::error_code ec, tcp::socket socket) {
                if (!ec) {
                    std::make_shared<Session>(std::move(socket))->start();
                }
                do_accept();
            });
    }

    tcp::acceptor acceptor_;
};

class Session : public std::enable_shared_from_this<Session> {
public:
    Session(tcp::socket socket) : socket_(std::move(socket)) {}

    void start() {
        do_read();
    }

private:
    void do_read() {
        auto self(shared_from_this());
        socket_.async_read_some(boost::asio::buffer(data_, max_length),
            [this, self](boost::system::error_code ec, std::size_t length) {
                if (!ec) {
                    do_write(length);
                }
            });
    }

    void do_write(std::size_t length) {
        auto self(shared_from_this());
        boost::asio::async_write(socket_, boost::asio::buffer(data_, length),
            [this, self](boost::system::error_code ec, std::size_t /*length*/) {
                if (!ec) {
                    do_read();
                }
            });
    }

    tcp::socket socket_;
    enum { max_length = 1024 };
    char data_[max_length];
};

int main() {
    try {
        boost::asio::io_context io_context;

        Server s(io_context, 12345);

        io_context.run();
    } catch (std::exception& e) {
        std::cerr << "Exception: " << e.what() << "\n";
    }

    return 0;
}
```

##### 예제 4: 비동기 타이머

```cpp
#include <iostream>
#include <boost/asio.hpp>

void print(const boost::system::error_code& /*e*/, boost::asio::steady_timer* t, int* count) {
    if (*count < 5) {
        std::cout << *count << "\n";
        ++(*count);

        t->expires_at(t->expiry() + boost::asio::chrono::seconds(1));
        t->async_wait(std::bind(print, std::placeholders::_1, t, count));
    }
}

int main() {
    boost::asio::io_context io_context;

    int count = 0;
    boost::asio::steady_timer t(io_context, boost::asio::chrono::seconds(1));
    t.async_wait(std::bind(print, std::placeholders::_1, &t, &count));

    io_context.run();

    std::cout << "Final count is " << count << "\n";

    return 0;
}
```

이 예제에서는 비동기 타이머를 사용하여 일정 시간 간격으로 콜백 함수를 호출하는 방법을 보여줍니다.
