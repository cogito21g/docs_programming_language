#### 10.3. Cocos2d-x

Cocos2d-x는 C++로 작성된 오픈 소스 게임 엔진으로, 2D 게임 개발에 최적화되어 있습니다. Cocos2d-x는 다양한 플랫폼을 지원하며, 게임 개발에 필요한 다양한 기능을 제공합니다.

##### Cocos2d-x 설치 및 설정

1. **Windows, macOS, Linux**:
   - [Cocos2d-x 다운로드 페이지](https://www.cocos.com/en/cocos2d-x/)에서 Cocos2d-x를 다운로드합니다.
   - 압축을 풀고, 설치된 디렉토리로 이동합니다.
   - `setup.py` 스크립트를 실행하여 환경 변수를 설정합니다:
     ```bash
     python setup.py
     source ~/.bash_profile  # macOS, Linux
     ```

2. **Cocos 프로젝트 생성**:
   - Cocos2d-x 명령줄 도구를 사용하여 새 프로젝트를 생성합니다:
     ```bash
     cocos new MyGame -p com.example.mygame -l cpp -d ./projects
     ```

##### 기본 사용법

Cocos2d-x를 사용하여 간단한 2D 게임 프로젝트 생성 및 실행:

1. **코드 작성**

   `HelloWorldScene.cpp` 파일을 열고 다음 코드를 작성합니다:

   ```cpp
   #include "HelloWorldScene.h"
   #include "SimpleAudioEngine.h"

   USING_NS_CC;

   Scene* HelloWorld::createScene()
   {
       return HelloWorld::create();
   }

   bool HelloWorld::init()
   {
       if (!Scene::init())
       {
           return false;
       }

       auto visibleSize = Director::getInstance()->getVisibleSize();
       Vec2 origin = Director::getInstance()->getVisibleOrigin();

       // 배경 생성
       auto background = LayerColor::create(Color4B(255, 255, 255, 255));
       this->addChild(background, -1);

       // 레이블 생성
       auto label = Label::createWithTTF("Hello, Cocos2d-x!", "fonts/Marker Felt.ttf", 24);
       if (label)
       {
           label->setPosition(Vec2(origin.x + visibleSize.width / 2,
                                   origin.y + visibleSize.height - label->getContentSize().height));
           this->addChild(label, 1);
       }

       // 스프라이트 생성
       auto sprite = Sprite::create("HelloWorld.png");
       if (sprite)
       {
           sprite->setPosition(Vec2(visibleSize.width / 2 + origin.x, visibleSize.height / 2 + origin.y));
           this->addChild(sprite, 0);
       }

       return true;
   }
   ```

2. **CMake 설정**

   Cocos2d-x는 자체 빌드 시스템을 사용하므로 CMake가 필요하지 않습니다. 생성된 프로젝트 디렉토리로 이동하여 `build` 명령을 실행합니다:

   ```bash
   cd projects/MyGame
   cocos run -p linux  # macOS는 `-p mac`, Windows는 `-p win32`
   ```

3. **빌드 및 실행**

   - 위의 명령을 실행하면 Cocos2d-x 프로젝트가 빌드되고 실행됩니다.
   - `Hello, Cocos2d-x!` 텍스트와 함께 기본 스프라이트가 표시됩니다.

##### 주요 기능

1. **크로스 플랫폼 지원**:
   - Windows, macOS, Linux, iOS, Android 등 다양한 플랫폼을 지원합니다.
2. **2D 그래픽**:
   - 2D 게임 개발에 최적화된 그래픽 엔진을 제공합니다.
3. **애니메이션**:
   - 스프라이트 애니메이션, 이동, 회전 등의 다양한 애니메이션 기능을 지원합니다.
4. **사운드**:
   - 배경 음악 및 효과음을 쉽게 추가할 수 있는 오디오 엔진을 제공합니다.
5. **물리 엔진**:
   - Box2D와 Chipmunk 물리 엔진을 통합하여 물리 기반 게임을 개발할 수 있습니다.

##### 예제 코드

간단한 2D 게임 프로젝트를 생성하고, 화면에 텍스트와 이미지를 표시하는 Cocos2d-x 코드입니다:

1. **코드 작성**

   `HelloWorldScene.cpp` 파일을 열고 다음 코드를 작성합니다:

   ```cpp
   #include "HelloWorldScene.h"
   #include "SimpleAudioEngine.h"

   USING_NS_CC;

   Scene* HelloWorld::createScene()
   {
       return HelloWorld::create();
   }

   bool HelloWorld::init()
   {
       if (!Scene::init())
       {
           return false;
       }

       auto visibleSize = Director::getInstance()->getVisibleSize();
       Vec2 origin = Director::getInstance()->getVisibleOrigin();

       // 배경 생성
       auto background = LayerColor::create(Color4B(255, 255, 255, 255));
       this->addChild(background, -1);

       // 레이블 생성
       auto label = Label::createWithTTF("Hello, Cocos2d-x!", "fonts/Marker Felt.ttf", 24);
       if (label)
       {
           label->setPosition(Vec2(origin.x + visibleSize.width / 2,
                                   origin.y + visibleSize.height - label->getContentSize().height));
           this->addChild(label, 1);
       }

       // 스프라이트 생성
       auto sprite = Sprite::create("HelloWorld.png");
       if (sprite)
       {
           sprite->setPosition(Vec2(visibleSize.width / 2 + origin.x, visibleSize.height / 2 + origin.y));
           this->addChild(sprite, 0);
       }

       return true;
   }
   ```

이 예제에서는 Cocos2d-x를 사용하여 기본적인 2D 게임 프로젝트를 생성하고, 화면에 텍스트와 이미지를 표시하는 간단한 프로그램을 구현합니다.
