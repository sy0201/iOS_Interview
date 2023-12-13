
### 앱이 시작할 때 main.c에 있는 UIApplicationMain함수에 의해 생성되는 객체는 무엇인가?

- UIApplication 싱글턴 객체가 생성됩니다.
> UIApplication이란? iOS에서 실행중인 앱의 제어와 조정을 담당하는 중앙지점입니다. 즉 여기서부터 실질적인 앱이라고 부를 수 있고, 해당 객체가 생성되고 나서야 개발자가 작성한 코드대로 이벤트 처리 등 앱의 동작을 제어할 수 있습니다.
> 모든 iOS앱은 단 하나의 UIApplication 인스턴스(또는 매우 드물게 UIApplication의 하위 클래스)가 있습니다. 앱이 시작되면 시스템은 UIApplicationMain(_:_:_:_:) 함수를 호출합니다.이 함수는 싱글턴 UIApplication 객체를 만들며, 그 후 공유 클래스 메서드를 호출하여 객체에 엑세스 합니다.

```swift
func UIApplicationMain(
    _ argc: Int32,
    _ argv: UnsafeMutablePointer<UnsafeMutablePointer<CChar>?>,
    _ principalClassName: String?,
    _ delegateClassName: String?
) -> Int32
```

[@main이란?](https://github.com/sy0201/iOS_Interview/blob/main/Main.md)







> https://developer.apple.com/documentation/uikit/uiapplication
> https://hyun083.tistory.com/85
> https://github.com/jwonyLee/TIL/blob/master/iOS/Interview/UIApplicationMain.md
> https://soooprmx.com/%ec%95%b1%eb%8d%b8%eb%a6%ac%ea%b2%8c%ec%9d%b4%ed%8a%b8-%ec%9d%b4%ed%95%b4%ed%95%98%ea%b8%b0-ios%ec%95%b1-%eb%a7%8c%eb%93%a4%ea%b8%b0-01/
