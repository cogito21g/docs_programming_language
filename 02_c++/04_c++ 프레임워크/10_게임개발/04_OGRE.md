#### 10.4. OGRE (Object-Oriented Graphics Rendering Engine)

OGRE는 C++로 작성된 강력한 3D 그래픽 렌더링 엔진으로, 복잡한 3D 그래픽 애플리케이션을 쉽게 개발할 수 있도록 다양한 기능을 제공합니다. OGRE는 다양한 그래픽 API를 지원하며, 고성능 렌더링을 위한 최적화된 엔진입니다.

##### OGRE 설치 및 설정

1. **Windows**:
   - [OGRE 다운로드 페이지](https://www.ogre3d.org/download/sdk)에서 Windows SDK를 다운로드하고 설치합니다.
   - Visual Studio를 사용하여 프로젝트를 설정합니다.

2. **macOS**:
   - Homebrew를 사용하여 OGRE를 설치합니다:
     ```bash
     brew install ogre
     ```

3. **Linux**:
   - 패키지 관리자를 사용하여 OGRE를 설치합니다:
     ```bash
     sudo apt-get install libogre-1.12-dev
     ```

##### 기본 사용법

OGRE를 사용하여 간단한 3D 장면을 렌더링:

1. **코드 작성**

   `main.cpp` 파일을 작성합니다:

   ```cpp
   #include <Ogre.h>
   #include <OgreApplicationContext.h>
   #include <OgreInput.h>
   #include <OgreRTShaderSystem.h>
   #include <OgreCameraMan.h>

   class BasicTutorial1 : public OgreBites::ApplicationContext, public OgreBites::InputListener {
   public:
       BasicTutorial1() : OgreBites::ApplicationContext("OgreTutorialApp") {}

       void setup() override {
           OgreBites::ApplicationContext::setup();
           addInputListener(this);

           Ogre::Root* root = getRoot();
           Ogre::SceneManager* scnMgr = root->createSceneManager();
           Ogre::RTShader::ShaderGenerator* shadergen = Ogre::RTShader::ShaderGenerator::getSingletonPtr();
           shadergen->addSceneManager(scnMgr);

           Ogre::SceneNode* rootNode = scnMgr->getRootSceneNode();

           // 카메라 설정
           Ogre::Camera* cam = scnMgr->createCamera("myCam");
           cam->setAutoAspectRatio(true);
           cam->setNearClipDistance(5);

           Ogre::SceneNode* camNode = rootNode->createChildSceneNode();
           camNode->attachObject(cam);

           getRenderWindow()->addViewport(cam);

           // 빛 설정
           Ogre::Light* light = scnMgr->createLight("MainLight");
           Ogre::SceneNode* lightNode = rootNode->createChildSceneNode();
           lightNode->setPosition(0, 10, 15);
           lightNode->attachObject(light);

           // 큐브 설정
           Ogre::Entity* ogreEntity = scnMgr->createEntity("ogrehead.mesh");
           Ogre::SceneNode* ogreNode = rootNode->createChildSceneNode();
           ogreNode->attachObject(ogreEntity);
       }

       bool keyPressed(const OgreBites::KeyboardEvent& evt) override {
           if (evt.keysym.sym == OgreBites::SDLK_ESCAPE) {
               getRoot()->queueEndRendering();
           }
           return true;
       }
   };

   int main(int argc, char** argv) {
       BasicTutorial1 app;
       app.initApp();
       app.getRoot()->startRendering();
       app.closeApp();
       return 0;
   }
   ```

2. **CMake 설정**

   `CMakeLists.txt` 파일을 작성합니다:

   ```cmake
   cmake_minimum_required(VERSION 3.10)
   project(OgreTutorial)

   set(CMAKE_CXX_STANDARD 11)

   find_package(OGRE REQUIRED)
   find_package(OIS REQUIRED)

   include_directories(${OGRE_INCLUDE_DIRS} ${OIS_INCLUDE_DIRS})
   link_directories(${OGRE_LIBRARY_DIRS})

   add_executable(OgreTutorial main.cpp)
   target_link_libraries(OgreTutorial ${OGRE_LIBRARIES} ${OIS_LIBRARIES})
   ```

3. **빌드 및 실행**

   ```bash
   mkdir build
   cd build
   cmake ..
   make
   ./OgreTutorial
   ```

##### 주요 기능

1. **고성능 3D 렌더링**:
   - 고성능의 3D 그래픽 렌더링을 위한 다양한 기능을 제공합니다.
2. **크로스 플랫폼 지원**:
   - Windows, macOS, Linux 등 다양한 플랫폼을 지원합니다.
3. **모듈화된 디자인**:
   - 모듈화된 디자인으로 필요한 기능만을 선택하여 사용할 수 있습니다.
4. **유연한 재질 시스템**:
   - 고급 재질 시스템을 통해 복잡한 셰이더와 텍스처를 관리할 수 있습니다.
5. **다양한 그래픽 API 지원**:
   - Direct3D, OpenGL 등 다양한 그래픽 API를 지원합니다.

##### 예제 코드

간단한 3D 장면을 렌더링하는 OGRE 코드입니다:

1. **코드 작성**

   `main.cpp` 파일을 작성합니다:

   ```cpp
   #include <Ogre.h>
   #include <OgreApplicationContext.h>
   #include <OgreInput.h>
   #include <OgreRTShaderSystem.h>
   #include <OgreCameraMan.h>

   class BasicTutorial1 : public OgreBites::ApplicationContext, public OgreBites::InputListener {
   public:
       BasicTutorial1() : OgreBites::ApplicationContext("OgreTutorialApp") {}

       void setup() override {
           OgreBites::ApplicationContext::setup();
           addInputListener(this);

           Ogre::Root* root = getRoot();
           Ogre::SceneManager* scnMgr = root->createSceneManager();
           Ogre::RTShader::ShaderGenerator* shadergen = Ogre::RTShader::ShaderGenerator::getSingletonPtr();
           shadergen->addSceneManager(scnMgr);

           Ogre::SceneNode* rootNode = scnMgr->getRootSceneNode();

           // 카메라 설정
           Ogre::Camera* cam = scnMgr->createCamera("myCam");
           cam->setAutoAspectRatio(true);
           cam->setNearClipDistance(5);

           Ogre::SceneNode* camNode = rootNode->createChildSceneNode();
           camNode->attachObject(cam);

           getRenderWindow()->addViewport(cam);

           // 빛 설정
           Ogre::Light* light = scnMgr->createLight("MainLight");
           Ogre::SceneNode* lightNode = rootNode->createChildSceneNode();
           lightNode->setPosition(0, 10, 15);
           lightNode->attachObject(light);

           // 큐브 설정
           Ogre::Entity* ogreEntity = scnMgr->createEntity("ogrehead.mesh");
           Ogre::SceneNode* ogreNode = rootNode->createChildSceneNode();
           ogreNode->attachObject(ogreEntity);
       }

       bool keyPressed(const OgreBites::KeyboardEvent& evt) override {
           if (evt.keysym.sym == OgreBites::SDLK_ESCAPE) {
               getRoot()->queueEndRendering();
           }
           return true;
       }
   };

   int main(int argc, char** argv) {
       BasicTutorial1 app;
       app.initApp();
       app.getRoot()->startRendering();
       app.closeApp();
       return 0;
   }
   ```

이 예제에서는 OGRE를 사용하여 간단한 3D 장면을 렌더링하고, 화면에 텍스트와 이미지를 표시하는 간단한 프로그램을 구현합니다.
