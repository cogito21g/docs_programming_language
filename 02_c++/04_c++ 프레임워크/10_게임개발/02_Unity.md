#### 10.2. Unity with C++

Unity는 주로 C#을 사용하는 강력한 게임 개발 엔진이지만, Unity에서 C++을 사용하는 경우도 있습니다. 특히, Unity는 네이티브 플러그인을 통해 C++ 코드를 사용할 수 있습니다. 이 기능을 사용하면 성능이 중요한 부분이나 기존 C++ 라이브러리를 Unity 프로젝트에서 활용할 수 있습니다.

##### Unity 설치 및 설정

1. **Unity 설치**:
   - [Unity 공식 웹사이트](https://unity.com/)에서 Unity Hub를 다운로드하여 설치합니다.
   - Unity Hub를 사용하여 최신 버전의 Unity Editor를 설치합니다.

2. **Visual Studio 설치**:
   - C++ 개발을 위해 Visual Studio를 설치합니다. 설치 시 `Desktop development with C++` 워크로드를 선택합니다.

##### 네이티브 플러그인 생성

Unity에서 C++ 코드를 사용하는 방법은 네이티브 플러그인을 만드는 것입니다.

1. **C++ DLL 프로젝트 생성**:

   - Visual Studio에서 새 프로젝트를 만듭니다.
   - `Windows Desktop Wizard` -> `Dynamic-link Library (DLL)`을 선택합니다.
   - 프로젝트 이름과 위치를 설정하고 `Create`를 클릭합니다.

2. **C++ 코드 작성**:

   - `DllMain` 함수와 간단한 함수 예제를 작성합니다.

   ```cpp
   #include <iostream>

   extern "C" {
       __declspec(dllexport) int Add(int a, int b) {
           return a + b;
       }

       __declspec(dllexport) const char* GetMessage() {
           return "Hello from C++!";
       }
   }
   ```

3. **DLL 빌드**:

   - 프로젝트를 빌드하여 DLL 파일을 생성합니다. 생성된 DLL 파일은 `Release` 또는 `Debug` 폴더에 있습니다.

##### Unity 프로젝트 설정

1. **Unity 프로젝트 생성**:

   - Unity Hub를 사용하여 새 프로젝트를 생성합니다.

2. **C++ DLL 파일 추가**:

   - 생성된 DLL 파일을 Unity 프로젝트의 `Assets/Plugins` 폴더에 복사합니다.

3. **C# 코드 작성**:

   - Unity에서 C# 스크립트를 작성하여 C++ DLL을 호출합니다.

   ```csharp
   using System;
   using System.Runtime.InteropServices;
   using UnityEngine;

   public class NativePluginTest : MonoBehaviour
   {
       // C++ 함수 선언
       [DllImport("YourDllName")]
       private static extern int Add(int a, int b);

       [DllImport("YourDllName")]
       private static extern IntPtr GetMessage();

       void Start()
       {
           // C++ 함수 호출
           int result = Add(3, 4);
           Debug.Log("Add(3, 4) = " + result);

           // 문자열 반환 함수 호출
           IntPtr messagePtr = GetMessage();
           string message = Marshal.PtrToStringAnsi(messagePtr);
           Debug.Log(message);
       }
   }
   ```

4. **Unity에서 스크립트 실행**:

   - Unity Editor에서 스크립트를 빈 게임 오브젝트에 추가하고, 게임을 실행하여 C++ 함수를 호출합니다.

##### 주요 기능

1. **고성능 연산**:
   - 성능이 중요한 부분을 C++로 구현하여 효율성을 높일 수 있습니다.
2. **기존 C++ 라이브러리 사용**:
   - 기존 C++ 라이브러리나 코드를 재사용할 수 있습니다.
3. **Unity와의 통합**:
   - 네이티브 플러그인을 통해 Unity와 C++ 코드를 통합할 수 있습니다.

##### 예제 코드

Unity와 C++ 네이티브 플러그인을 사용하는 예제입니다.

1. **C++ 코드 작성**:

   ```cpp
   #include <iostream>

   extern "C" {
       __declspec(dllexport) int Add(int a, int b) {
           return a + b;
       }

       __declspec(dllexport) const char* GetMessage() {
           return "Hello from C++!";
       }
   }
   ```

2. **C# 코드 작성**:

   ```csharp
   using System;
   using System.Runtime.InteropServices;
   using UnityEngine;

   public class NativePluginTest : MonoBehaviour
   {
       // C++ 함수 선언
       [DllImport("YourDllName")]
       private static extern int Add(int a, int b);

       [DllImport("YourDllName")]
       private static extern IntPtr GetMessage();

       void Start()
       {
           // C++ 함수 호출
           int result = Add(3, 4);
           Debug.Log("Add(3, 4) = " + result);

           // 문자열 반환 함수 호출
           IntPtr messagePtr = GetMessage();
           string message = Marshal.PtrToStringAnsi(messagePtr);
           Debug.Log(message);
       }
   }
   ```

이 예제에서는 Unity에서 네이티브 플러그인을 사용하여 C++로 작성된 함수를 호출합니다. 이를 통해 Unity 프로젝트에서 고성능 연산이나 기존 C++ 라이브러리를 활용할 수 있습니다.
