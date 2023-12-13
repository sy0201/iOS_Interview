
### @main이란?

- 프로그램의 시작점(Entry Point)으로 타입을 지정하기 위한 Swift 언어의 기능입니다.
- 어디서부터 이 앱이 최초 실행되고 시작될지를 명시해주는 키워드입니다.

- Xcode12 이전버전에서는 @UIApplicationMain으로 되어있고 Xcode12 이후 버전부터 @main으로 바뀌었습니다. 
- Swift 컴파일러가 @main이라는 annotation을 통해 AppDelegate에서 전역범위에 있는 코드를 자동으로 인식하게 하고 실행시켜 Entry Point가 지정되게 합니다.


그럼 @UIApplicationMain은 Xcode12이후에서 사용이 가능할까요?
 - YES! 앱의 라이프사이클을 관리하기 위한 UIApplication 객체생성을 위한 키워드이기때문에 빌드에는 이상이 없다! 하지만 class에서만 선언될수 있어서 SwiftUI에서는 에러가 발생
- @UIApplicationMain은 UIApplicationMain 함수를 호출하고 해당 클래스의 이름을 델리게이트 클래스의 이름으로 전달합니다.
> UIApplicationMain 함수는 앱 실행에서 중요한 몇가지 기능을 수행합니다.
>- 앱의 본체에 해당하는 객체인 UIApplication객체를 생성, 이 객체는 앱의 Life Cycle을 관리합니다.
> - 지정된 클래스(@UIApplicationMain)에서 델리게이트를 인스턴스화 하고 이를 앱의 객체에 할당합니다.
> - 앱의 Run Loop를 포함한 기본 이벤트 처리 루프를 설정하고 이벤트 처리를 시작합니다.
> - 앱의 info.plist에 불러올 main nib파일이 제대로 명시되어있으면 해당 nib파일을 불러옵니다.(User Interface를 불러옵니다.)


왜 @main으로 바뀌었을까?
- 사용자는 탑레벨의 코드를 작성하는 대신 @main 단일 유형의 속성을 사용할 수 있고, 라이브러리와 프레임워크는 프로토콜이나 클래스 상속을 통해 맞춤형 진입점 동작을 제공할 수 있다.


> Swift 소스파일의 Top-Level Code는 0개 이상의 명령문, 선언 및 표현식으로 구성됩니다. 기본적으로 소스파일의 Top-Level Code에서 선언된 변수, 상수 및 그 외 선언은 동일 모듈의 일부인 모든 소스파일의 코드에서 엑세스할 수 있다고 합니다. 또한 기본동작을 재정의 할 수 있습니다.
> Top-Level Code는 선언문(top-level declarations)과 실행문 (excutable top-level code) 두가지로 나뉘며, Top-Level 선언은 모든 Swift 소스 파일에서 허용됩니다. 실행 가능한 Top-Level Code는 선언과 명령문 및 표현식이 포함되며, 프로그램 최상위 진입점 즉, Entry Point로만 허용됩니다.

- object-c 일때 main.m 소스파일에 main()함수가 있었고, 이 함수로 앱의 진입점을 나타내며 시작이 되었습니다. 그러다 Swift로 넘어오면서 main.swift가 동일한 역할을 해주게 됩니다. 동시에 UIKit framework 안에 이 main 함수를 넣어 관리하고 우리는 @UIApplicationMain 키워드만 사용하게 된것입니다. 자동으로 컴파일하면서 해당 키워드를 찾고 인식하고있던것입니다. 

[앱이 시작할 때 main.c에 있는 UIApplicationMain함수에 의해 생성되는 객체는 무엇인가? 자세히보기](https://github.com/sy0201/iOS_Interview/blob/main/UIApplicationMain.md)

```swift
func UIApplicationMain(
    _ argc: Int32,
    _ argv: UnsafeMutablePointer<UnsafeMutablePointer<CChar>?>,
    _ principalClassName: String?,
    _ delegateClassName: String?
) -> Int32
```
클래스 이름으로 파라미터를 받고 있는데, 클래스에서 사용될 수 있는 UIApplicationMain()함수일 것이며, 이 함수는(UIApplicationMain()) 실제 UIApplication 객체를 생성하게 됩니다. @UIApplicationMAin 키워드는 클래스에서만 사용가능한것이 @Main을 사용하게 된 이유가 될 수 있을것입니다.
이러한 문제는 타입 기반의 Swift 코드에서 오류가 있었고 main()함수 자체가 static 메서드(타입메서드) 이기에 protocol에서 확장 메서드 및 기본 클래스로 제공될 수 있습니다.
즉 커스텀하게 원하는 곳에서 @main을 통해 진입점을 제공해줄 수 있다는 뜻입니다.

> @main을 사용함으로써 타입 기반의 코드에서 더 알맞은 앱 진입접을 표현해줄 수 있고, 타입메서드인 static func main()을 통해 확장 및 기본 클래스로 제공되며 사용할 수 있는 장점을 가지게 됩니다.




> https://developer.apple.com/documentation/uikit/1622933-uiapplicationmain
> https://barosalki.tistory.com/entry/iOS-Swift-53-main-type%EA%B8%B0%EB%B0%98%EC%9D%98-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%A8-%EC%A7%84%EC%9E%85%EC%A0%90
> https://green1229.tistory.com/265
> https://seungchan.tistory.com/79
