
### Notification이란? (Delegation Design Pattern)

- 알림센터를 통해 등록된 모든 관찰자에게 정보가 방송되는 컨테이너입니다. 
- 무언가 액션이 있으면 이 액션이 진행되었으니 담당자들은 처리해주세요~ 하는 곳!
- NotificationCenter를 통해 정보를 저장하기 위한 구조체다.
- 유저에게 알리는 역할이 아닌 애플리케이션 내에서 정보를 전하기 위한 역할로 어디로 송신하고싶은지 Notification의 전달하는 중심 역할은 NotificationCenter

```swift
    var name: Notification.Name
    var object: Any?
    var userInfo: [AnyHashable: Any]?
```
name: 전달하고자 하는 notification의 이름(name을 통해 알림을 식별)
object: 전달하고자 하는 데이터(객체)
userInfo: notification과 관련된 값 (=extra data를 보내는데 사용 가능)

### NotificationCenter

- 어떠한 이벤트에 대해 구독하고, 해당 이벤트가 일어나면 알림을 주는 역할이 NotificationCenter입니다.
- NotificationCenter는 URLSession, UserDefault와 같이 default를 통해 싱글톤을 사용할 수 있습니다.
- Notification이 오면 Observer Pattern을 통해서 등록된 Observer들에게 Notification을 전달하기 위해 사용하는 클래스입니다.
> 등록된 관찰자에게 정보를 방송할 수 있는 알림 방송 메커니즘(‘Notification Dispatch’)입니다.
 NotificationDispatch란? 여기서 Dispatch는 어떤걸 전송하는 역할을 담고 있는 것을 의미한다. 예를 들면 DispatchQueue는 thread에 어떠한 작업을 해달라고 보낼것이 모인게 Queue이다. Dispatch를 사용하는 이유는 데이터의 흐름의 한 지점을 지나게 할 수 있다는 장점이 있기 때문이다.

NotificationCenter에 Observer등록하기
```swift
NotificationCenter.default.addObserver(self,selector: #selector("실행할 함수"),
                                       name: Notification.Name("신호이름"),
                                       object : nil)
```
### Notification 사용법

- Notification의 전달은 알림을 만들어내는 Publisher, 만들어진 알림을 전달하는 Dispatcher역할의 NotificationCenter, 그 알림을 관찰하는 Observer(혹은 Subscriber)의 관계를 통해 진행된다.

```swift
// 1) Notification을 보내는 ViewController
class PostViewController: UIViewController {

    @IBOutlet weak var sendNotificationButton: UIButton!

    override func viewDidLoad() {
        super.viewDidLoad()
        setupButtonTarget()
    }

    func setupButtonTarget() {
        sendNotificationButton.addTarget(self, action: #selector(sendNotificationTapped), for: .touchUpInside)
    }

    @objc func sendNotificationTapped() {
        guard let backgroundColor = view.backgroundColor else { return }

        NotificationCenter.default.post(name: Notification.Name("notification"),
                                        object: sendNotificationButton,
                                        userInfo: ["backgroundColor": backgroundColor])
    }
}

class ReceivingViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        NotificationCenter.default.addObserver(self,
                                               selector: #selector(receivedNotification),
                                               name: Notification.Name("notification"),
                                               object: nil)
    }

    @objc func receivedNotification(notification: Notification) {
        guard let notificationObject = notification.object as? UIButton else { return }
        print(notificationObject.titleLabel?.text ?? "Text is Empty")

        guard let notificationUserInfo = notification.userInfo as? [String: UIColor],
              let postViewBackgroundColor = notificationUserInfo["backgroundColor"] else { return }
        print(postViewBackgroundColor)
    }
}

```

### Notification 장단점
장점
- 다수의 객체들에게 동시에 이벤트의 발생을 알려줄 수 있다.
- 많은 줄의 코드가 필요없이 쉽게 구현이 가능
- Notification과 관련된 정보를 Any? 타입의 object, [AnyHashable: Any]? 타입의 userInfo로 전달할 수 있다.
> Any? 및 [AnyHashable: Any]?을 사용하면 거의 모든 종류의 데이터를 전달 할 수 있어 다양한 데이터 형식 및 객체 유형을 처리할 수 있습니다.
> Any? 및 [AnyHashable: Any]? 은 Object-C와의 상호운용성을 제공하여 앱의 다양한 부분에서 통신이 가능해집니다.
> userInfo를 사용하면 여러데이터를 하나의 알림에 포함하여 전달 할 수 있어 여러개의 데이터를 전달해야하는 상황에 유용합니다.
> Any? 타입을 사용하여 유연성을 확보하면서도 타입안정성을 일부 유지할 수 있습니다.
단점
- key 값으로 Notification의 이름과 userInfo를 서로 맞추기 때문에 컴파일 시 구독이 잘 되고 있는지, 올바르게 userInfo의 value를 받아오는지 체크가 어렵다.
- 컴파일 추적이 어렵다.
- Notification post 이후 정보를 받을 수 없다.


### NotificationCenter는 언제 사용해야할까?
- 앱 내에서 공식적인 연결이 없는 두 개 이상의 컴포넌트들이 상호작용이 필요할 때
- 상호작용이 반복적으로 그리고 지속적으로 이루어져야할때
- 일대다, 다대다 통신을 사용하는 경우

### deinit을 통한 구독취소를 꼭 해줘야 합니다.(iOS9 이상부터 자동해제)
Observer는 계속 살아있기때문에 화면이 꺼질때, 메모리 낭비를 막기위해 해제를 해줘야 합니다.

```swift
notificationCenter.removeObserver(self)
```



> https://jiseobkim.github.io/swift/2018/10/27/swift-NotificationCenter.html
> https://medium.com/@Alpaca_iOSStudy/delegation-notification-%EA%B7%B8%EB%A6%AC%EA%B3%A0-kvo-82de909bd29
> https://leeari95.tistory.com/49
> https://medium.com/hcleedev/swift-notificationcenter%EC%99%80-%EC%82%AC%EC%9A%A9%EB%B2%95-6eb4490aac88
