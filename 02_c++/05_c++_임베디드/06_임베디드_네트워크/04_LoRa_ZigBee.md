#### 6.4. LoRa 및 ZigBee

LoRa와 ZigBee는 저전력, 장거리 무선 통신을 위한 프로토콜로, 주로 IoT(Internet of Things) 애플리케이션에서 사용됩니다. 이들 프로토콜은 저전력 소비와 견고한 네트워크 형성을 통해 다양한 센서 네트워크와 스마트 시티, 농업 모니터링 등의 분야에서 널리 활용됩니다.

##### LoRa (Long Range)

LoRa는 장거리 무선 통신 기술로, 주로 LoRaWAN 프로토콜과 함께 사용됩니다. LoRaWAN은 물리 계층(LoRa)과 네트워크 계층(LoRaWAN)을 결합한 통신 표준으로, 저전력, 장거리, 높은 네트워크 용량을 제공합니다.

**LoRa의 주요 특징**:
- **장거리 통신**: 수 킬로미터 이상의 통신 범위를 제공합니다.
- **저전력 소비**: 배터리 수명이 몇 년에 이를 수 있습니다.
- **깊은 건물 침투력**: 도시 환경에서도 강력한 신호 전파를 제공합니다.
- **비인가 주파수 대역 사용**: ISM(Industrial, Scientific, and Medical) 밴드 사용으로 면허가 필요 없습니다.

**예제: Arduino와 LoRa 모듈 사용**:
다음 예제에서는 Arduino와 LoRa 모듈을 사용하여 데이터를 전송하는 방법을 설명합니다.

**필요한 하드웨어**:
- Arduino 보드 (예: Arduino UNO)
- LoRa 모듈 (예: SX1278 LoRa 모듈)
- 연결선

**예제 코드**:
```cpp
#include <SPI.h>
#include <LoRa.h>

void setup() {
    Serial.begin(9600);
    while (!Serial);

    Serial.println("LoRa Sender");

    if (!LoRa.begin(915E6)) { // 주파수 설정 (예: 915 MHz)
        Serial.println("Starting LoRa failed!");
        while (1);
    }
}

void loop() {
    Serial.print("Sending packet: ");
    Serial.println("Hello LoRa!");

    // 패킷 전송
    LoRa.beginPacket();
    LoRa.print("Hello LoRa!");
    LoRa.endPacket();

    delay(1000);
}
```

**예제 설명**:
- **LoRa 초기화**: `LoRa.begin()` 함수를 사용하여 LoRa 모듈을 초기화하고 주파수를 설정합니다.
- **데이터 전송**: `LoRa.beginPacket()`, `LoRa.print()`, `LoRa.endPacket()` 함수를 사용하여 데이터를 전송합니다.

##### ZigBee

ZigBee는 저전력, 저속 무선 메쉬 네트워크 프로토콜로, 주로 스마트 홈, 산업 자동화, 무선 센서 네트워크 등에 사용됩니다. ZigBee는 IEEE 802.15.4 표준을 기반으로 하며, 안정적이고 확장 가능한 네트워크를 형성할 수 있습니다.

**ZigBee의 주요 특징**:
- **저전력 소비**: 배터리 수명이 몇 년에 이를 수 있습니다.
- **메쉬 네트워크**: 여러 노드가 서로 연결되어 네트워크를 형성하고, 장애 노드가 생겨도 자동으로 경로를 재설정합니다.
- **비인가 주파수 대역 사용**: 2.4GHz ISM 밴드 사용으로 면허가 필요 없습니다.
- **표준화된 프로토콜**: 다양한 제조사의 장치 간 상호 운용성을 보장합니다.

**예제: XBee 모듈을 사용한 ZigBee 통신**:
다음 예제에서는 XBee 모듈을 사용하여 데이터를 전송하는 방법을 설명합니다.

**필요한 하드웨어**:
- Arduino 보드 (예: Arduino UNO)
- XBee 모듈 (예: XBee S2C)
- XBee 아답터 보드
- 연결선

**예제 코드**:
```cpp
#include <SoftwareSerial.h>

SoftwareSerial xbee(2, 3); // RX, TX

void setup() {
    Serial.begin(9600);
    xbee.begin(9600);

    Serial.println("ZigBee Sender");
}

void loop() {
    Serial.println("Sending packet: Hello ZigBee!");
    xbee.println("Hello ZigBee!");
    delay(1000);
}
```

**예제 설명**:
- **XBee 초기화**: `SoftwareSerial`을 사용하여 XBee 모듈과 통신을 설정합니다.
- **데이터 전송**: `xbee.println()` 함수를 사용하여 데이터를 전송합니다.

##### LoRa와 ZigBee 비교

| 특징                  | LoRa                          | ZigBee                        |
|-----------------------|-------------------------------|-------------------------------|
| **통신 범위**         | 수 킬로미터 이상              | 수십 미터                    |
| **전송 속도**         | 낮음 (수십 kbps)              | 중간 (250 kbps)              |
| **전력 소비**         | 매우 낮음                     | 낮음                         |
| **네트워크 토폴로지**  | 스타 토폴로지, 메쉬 네트워크 | 메쉬 네트워크                |
| **주파수 대역**       | ISM 밴드 (비인가 주파수)      | 2.4GHz ISM 밴드              |
| **응용 분야**         | 스마트 시티, 농업 모니터링    | 스마트 홈, 산업 자동화       |

##### 주요 사용 사례

**LoRa**:
- **스마트 시티**: 환경 모니터링, 스마트 조명, 쓰레기 관리 등.
- **농업**: 토양 습도, 기상 관측, 농작물 모니터링 등.
- **원격 위치 추적**: 장거리 위치 추적 시스템.

**ZigBee**:
- **스마트 홈**: 조명 제어, 보안 시스템, 스마트 플러그 등.
- **산업 자동화**: 무선 센서 네트워크, 공정 제어 시스템 등.
- **의료**: 환자 모니터링 시스템.

LoRa와 ZigBee는 각각의 특성과 용도에 맞게 선택하여 임베디드 시스템에서 효율적으로 사용할 수 있습니다. 각 기술의 설정 방법과 프로그래밍 기법을 잘 이해하고 활용하는 것이 중요합니다.
