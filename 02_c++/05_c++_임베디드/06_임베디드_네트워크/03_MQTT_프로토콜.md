#### 6.3. MQTT 프로토콜

MQTT (Message Queuing Telemetry Transport)는 경량의 메시지 프로토콜로, 주로 IoT(Internet of Things) 및 M2M(Machine-to-Machine) 통신에 사용됩니다. MQTT는 퍼블리셔-서브스크라이버(Publish-Subscribe) 모델을 사용하여 장치 간의 데이터 전송을 효율적으로 관리합니다.

##### MQTT의 주요 특징

1. **경량 프로토콜**: 최소한의 네트워크 대역폭과 리소스를 사용하여 저전력 장치와 불안정한 네트워크에서도 안정적으로 동작합니다.
2. **퍼블리셔-서브스크라이버 모델**: 클라이언트는 브로커(Broker)를 통해 메시지를 주고받으며, 직접 통신하지 않습니다.
3. **QoS(Quality of Service) 레벨**: 다양한 QoS 레벨을 제공하여 메시지 전송의 신뢰성을 조절할 수 있습니다.
4. **유연한 토픽 구조**: 토픽을 계층적으로 구조화하여 데이터를 구독하고 필터링할 수 있습니다.

##### MQTT의 구성 요소

1. **브로커(Broker)**: 모든 메시지를 중개하며, 퍼블리셔와 서브스크라이버 간의 통신을 관리합니다.
2. **퍼블리셔(Publisher)**: 특정 토픽으로 메시지를 전송하는 클라이언트입니다.
3. **서브스크라이버(Subscriber)**: 특정 토픽을 구독하고, 해당 토픽으로 전달된 메시지를 수신하는 클라이언트입니다.

##### QoS (Quality of Service) 레벨

1. **QoS 0 (At most once)**: 메시지가 한 번만 전송되며, 전송 성공 여부를 보장하지 않습니다.
2. **QoS 1 (At least once)**: 메시지가 적어도 한 번 전송되며, 중복 메시지가 발생할 수 있습니다.
3. **QoS 2 (Exactly once)**: 메시지가 정확히 한 번 전송되며, 중복 없이 전송을 보장합니다.

##### MQTT 사용 예제

다음 예제에서는 ESP32를 사용하여 MQTT 브로커에 연결하고, 데이터를 퍼블리시(Publish)하고 구독(Subscribe)하는 방법을 설명합니다.

**1. Arduino IDE 설정**

먼저 Arduino IDE에 PubSubClient 라이브러리를 설치해야 합니다. 이 라이브러리는 MQTT 프로토콜을 지원하며, 간편하게 MQTT 통신을 구현할 수 있습니다.

**2. 예제 코드 작성**

**ESP32에서 MQTT 사용 예제 코드**:
```cpp
#include <WiFi.h>
#include <PubSubClient.h>

// WiFi 설정
const char* ssid = "your_SSID";
const char* password = "your_PASSWORD";

// MQTT 브로커 설정
const char* mqtt_server = "broker.hivemq.com";

WiFiClient espClient;
PubSubClient client(espClient);

void setup() {
    Serial.begin(115200);
    setup_wifi();
    client.setServer(mqtt_server, 1883);
    client.setCallback(callback);
}

void setup_wifi() {
    delay(10);
    Serial.println();
    Serial.print("Connecting to ");
    Serial.println(ssid);

    WiFi.begin(ssid, password);

    while (WiFi.status() != WL_CONNECTED) {
        delay(1000);
        Serial.print(".");
    }

    Serial.println("");
    Serial.println("WiFi connected");
    Serial.println("IP address: ");
    Serial.println(WiFi.localIP());
}

void callback(char* topic, byte* payload, unsigned int length) {
    Serial.print("Message arrived [");
    Serial.print(topic);
    Serial.print("] ");
    for (unsigned int i = 0; i < length; i++) {
        Serial.print((char)payload[i]);
    }
    Serial.println();
}

void reconnect() {
    while (!client.connected()) {
        Serial.print("Attempting MQTT connection...");
        if (client.connect("ESP32Client")) {
            Serial.println("connected");
            client.subscribe("test/topic");
        } else {
            Serial.print("failed, rc=");
            Serial.print(client.state());
            Serial.println(" try again in 5 seconds");
            delay(5000);
        }
    }
}

void loop() {
    if (!client.connected()) {
        reconnect();
    }
    client.loop();

    // 데이터 퍼블리시
    String message = "Hello from ESP32";
    client.publish("test/topic", message.c_str());
    delay(1000);
}
```

**3. 코드 설명**

- **WiFi 설정**: `setup_wifi()` 함수를 사용하여 WiFi에 연결합니다.
- **MQTT 설정**: `client.setServer()`를 사용하여 MQTT 브로커를 설정하고, `client.setCallback()`을 사용하여 메시지 수신 콜백을 설정합니다.
- **재연결**: `reconnect()` 함수를 사용하여 MQTT 브로커에 연결합니다. 연결이 끊어지면 재연결을 시도합니다.
- **메시지 퍼블리시**: `client.publish()`를 사용하여 특정 토픽으로 메시지를 퍼블리시합니다.
- **메시지 수신**: `callback()` 함수는 메시지가 도착할 때 호출되며, 도착한 메시지를 처리합니다.

##### MQTT의 주요 사용 사례

1. **IoT 장치 간 통신**: MQTT는 IoT 장치 간의 효율적인 데이터 통신을 가능하게 하며, 저전력 장치에서도 안정적으로 동작합니다.
2. **원격 모니터링 및 제어**: MQTT를 사용하여 센서 데이터를 수집하고, 원격으로 장치를 제어할 수 있습니다.
3. **실시간 데이터 스트리밍**: MQTT는 실시간 데이터 스트리밍에 적합하여, 실시간으로 데이터를 수집하고 분석할 수 있습니다.

##### 보안 고려 사항

MQTT는 경량 프로토콜이므로 기본적으로 보안 기능이 약합니다. 다음과 같은 보안 메커니즘을 추가하여 통신의 안전성을 높일 수 있습니다:

1. **TLS/SSL 암호화**: MQTT 메시지를 TLS/SSL로 암호화하여 데이터 전송 중에 발생할 수 있는 도청을 방지합니다.
2. **인증 및 권한 부여**: MQTT 브로커에서 클라이언트 인증과 권한 부여를 설정하여, 신뢰할 수 있는 클라이언트만 접속할 수 있도록 합니다.
3. **방화벽 및 네트워크 보안**: MQTT 브로커가 있는 서버에 방화벽을 설정하여, 불필요한 네트워크 접근을 차단합니다.

MQTT는 임베디드 시스템과 IoT 환경에서 효율적이고 신뢰성 있는 데이터 통신을 제공하는 강력한 도구입니다. 각 프로젝트의 요구 사항에 맞게 MQTT의 기능을 잘 활용하여 안정적이고 효율적인 통신을 구현하는 것이 중요합니다.
