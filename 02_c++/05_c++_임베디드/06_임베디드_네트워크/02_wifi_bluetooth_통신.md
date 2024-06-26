#### 6.2. Wi-Fi 및 Bluetooth 통신

Wi-Fi와 Bluetooth는 임베디드 시스템에서 널리 사용되는 무선 통신 기술입니다. 이 두 기술은 서로 다른 용도와 특성을 가지고 있으며, 각각의 요구 사항에 맞게 선택하여 사용됩니다.

##### Wi-Fi 통신

Wi-Fi는 무선 로컬 영역 네트워크(WLAN) 기술로, 임베디드 시스템이 인터넷에 연결되거나 로컬 네트워크에서 통신할 수 있도록 합니다. Wi-Fi는 높은 데이터 전송 속도와 넓은 범위를 제공하며, IoT 장치, 스마트 홈, 산업 자동화 등 다양한 응용 분야에서 사용됩니다.

**Wi-Fi의 주요 특징**:
- **높은 데이터 전송 속도**: 최대 수백 Mbps의 데이터 전송 속도를 제공합니다.
- **넓은 통신 범위**: 최대 수십 미터까지 통신이 가능합니다.
- **인터넷 연결**: 임베디드 시스템이 인터넷에 연결되어 클라우드 서비스와 통신할 수 있습니다.

**예제: ESP32에서 Wi-Fi 설정 및 사용**:
ESP32는 Wi-Fi 및 Bluetooth를 모두 지원하는 강력한 마이크로컨트롤러입니다. 여기서는 ESP32에서 Wi-Fi를 설정하고 사용하는 방법을 설명합니다.

**예제 코드**:
```cpp
#include <WiFi.h>

const char* ssid = "your_SSID";
const char* password = "your_PASSWORD";

void setup() {
    Serial.begin(115200);
    WiFi.begin(ssid, password);

    while (WiFi.status() != WL_CONNECTED) {
        delay(1000);
        Serial.println("Connecting to WiFi...");
    }

    Serial.println("Connected to WiFi");
    Serial.println("IP address: ");
    Serial.println(WiFi.localIP());
}

void loop() {
    // Wi-Fi를 통해 데이터를 전송하거나 수신하는 코드
}
```

##### Bluetooth 통신

Bluetooth는 저전력 근거리 무선 통신 기술로, 주로 웨어러블 기기, 무선 센서 네트워크, 오디오 장치 등에 사용됩니다. Bluetooth는 낮은 전력 소비와 쉬운 페어링 과정을 제공하여 근거리 통신에 적합합니다.

**Bluetooth의 주요 특징**:
- **저전력 소비**: 저전력 모드를 사용하여 배터리 수명을 연장할 수 있습니다.
- **쉬운 페어링**: 간단한 페어링 과정을 통해 장치 간 연결을 설정할 수 있습니다.
- **근거리 통신**: 약 10미터 이내의 근거리 통신에 적합합니다.

**예제: ESP32에서 Bluetooth 설정 및 사용**:
ESP32는 Bluetooth Classic과 Bluetooth Low Energy(BLE)를 모두 지원합니다. 여기서는 BLE를 사용하여 데이터를 전송하는 방법을 설명합니다.

**예제 코드**:
```cpp
#include <BLEDevice.h>
#include <BLEUtils.h>
#include <BLEServer.h>

BLEServer* pServer = NULL;
BLECharacteristic* pCharacteristic = NULL;
bool deviceConnected = false;

const char* serviceUUID = "12345678-1234-1234-1234-123456789abc";
const char* characteristicUUID = "abcdefab-cdef-abcd-efab-cdefabcdefab";

void setup() {
    Serial.begin(115200);

    // BLE 초기화
    BLEDevice::init("ESP32_BLE");
    pServer = BLEDevice::createServer();
    pServer->setCallbacks(new MyServerCallbacks());

    BLEService* pService = pServer->createService(serviceUUID);
    pCharacteristic = pService->createCharacteristic(
        characteristicUUID,
        BLECharacteristic::PROPERTY_READ |
        BLECharacteristic::PROPERTY_WRITE
    );

    pCharacteristic->setValue("Hello BLE");
    pService->start();

    // BLE 광고 시작
    BLEAdvertising* pAdvertising = BLEDevice::getAdvertising();
    pAdvertising->addServiceUUID(serviceUUID);
    pAdvertising->setScanResponse(true);
    pAdvertising->setMinPreferred(0x06);
    pAdvertising->setMinPreferred(0x12);
    BLEDevice::startAdvertising();
    Serial.println("Waiting for a client connection to notify...");
}

void loop() {
    if (deviceConnected) {
        // 데이터 전송
        pCharacteristic->setValue("Hello from ESP32");
        pCharacteristic->notify();
        delay(1000);
    }
}

class MyServerCallbacks: public BLEServerCallbacks {
    void onConnect(BLEServer* pServer) {
        deviceConnected = true;
    };

    void onDisconnect(BLEServer* pServer) {
        deviceConnected = false;
    }
};
```

##### Wi-Fi와 Bluetooth 비교

| 특징                  | Wi-Fi                        | Bluetooth                       |
|-----------------------|------------------------------|---------------------------------|
| **전송 속도**         | 수백 Mbps                    | 최대 3 Mbps (Bluetooth 5.0)     |
| **전력 소비**         | 비교적 높음                  | 저전력 모드 제공                |
| **통신 범위**         | 수십 미터                    | 약 10미터                       |
| **응용 분야**         | 인터넷 연결, 로컬 네트워크   | 웨어러블 기기, 무선 센서 네트워크 |
| **페어링**            | 필요 없음 (AP 연결)          | 필요 (간단한 페어링)            |
| **데이터 보안**       | WPA, WPA2 등                 | AES 암호화                      |

##### 임베디드 시스템에서 Wi-Fi와 Bluetooth의 주요 고려사항

1. **전력 소비**: 배터리로 구동되는 시스템에서는 Bluetooth의 저전력 모드를 활용하거나 Wi-Fi의 전력 관리 기능을 사용해야 합니다.
2. **통신 범위**: 통신 범위에 따라 적절한 기술을 선택해야 합니다. Wi-Fi는 넓은 범위를 커버할 수 있으며, Bluetooth는 근거리 통신에 적합합니다.
3. **데이터 전송 속도**: 높은 데이터 전송 속도가 필요한 응용에서는 Wi-Fi를, 낮은 데이터 전송 속도로 충분한 응용에서는 Bluetooth를 사용합니다.
4. **응용 분야**: IoT 장치, 스마트 홈 등 인터넷 연결이 필요한 경우 Wi-Fi를, 웨어러블 기기나 무선 센서 네트워크 등 근거리 통신이 필요한 경우 Bluetooth를 사용합니다.

Wi-Fi와 Bluetooth는 각각의 특성과 용도에 맞게 선택하여 임베디드 시스템에서 효율적으로 사용할 수 있습니다. 각 기술의 설정 방법과 프로그래밍 기법을 잘 이해하고 활용하는 것이 중요합니다.
