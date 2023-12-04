
### Delegate란? (Delegation Design Pattern)

Delegate란 대리자라는 뜻으로 어떤 객체가 모든 일을 처리하는 것이 아니라 처리해야 할 일 중 일부를 프로토콜을 채택하여 대신 처리해주는 역할을 수행합니다.

- 먼저 전달 받을 ViewController(SecondViewController) 작성: Protocol 작성
```swift
// 1. 함수 원형 protocol 만 작성하고 함수 구현 부는 전달 보내는 ViewController에서 작성
protocol SendProtocol {
  func dataSend(data: String)
}

class SecondViewController: UIViewController {

  @IBOutlet weak var dataTextField: UITextField!
  
  //2. SendProtocol형의 delegate 프로퍼티 생성
  weak var delegate: SendProtocol?
  
  override func viewDidLoad() {
        super.viewDidLoad()
    }
    
  @IBAction func dataSendButtonTapped(_ sender: Any) {
    //3. 버튼을 눌렀을 때, 선언한 delegate의 dataSend에 textField의 텍스트를 담는 이벤트를 동작하게함
    if let text = dataTextField.text{
      delegate?.dataSend(data: text)
    }
    
    //4. delegate 처리가 끝난 뒤에, navigation pop처리
    self.navigationController?.popViewController(animated: true)
  }
}
```

```swift
class FirstViewController: UIViewController, SendProtocol {
  
  @IBOutlet weak var dataLabel: UILabel!
  
  override func viewDidLoad() {
    super.viewDidLoad()
  }

  // 2. protocol의 함수 구현부 작성
  func dataSend(data: String) {
    dataLabel.text = data
  }
  
  @IBAction func nextButtonTapped(_ sender: Any) {
    guard let nextVC = self.storyboard?.instantiateViewController(withIdentifier: "SecondViewController") as? SecondViewController else { return }
    
    // 3. SecondViewController에서 선언해둔 delegate가 self. 대리자는 FirstViewController
    nextVC.delegate = self
    self.navigationController?.pushViewController(nextVC, animated: true)
  }
}
```


### Delegate는 언제 사용할까?

- AViewController 위에 tableViewCell이나 collectionViewCell 등을 올릴때, TableViewController, CollectionViewController를 품고 있는 상위 ViewController에 데이터를 전달 할 때 사용합니다.

- 나중에 메모리에 올라오는 ViewController에서 -> 먼저 메모리에 올라와 있는 ViewController 상태에서 데이터 전달할 때 사용되는 방식  

- 보통 push를 하면서 데이터 넘길때 segue 방식으로 데이터를 넘기는데, 반대로 pop 될때는 ViewController가 dismiss(메모리에서 해제)되면서 데이터가 유실되기때문에 segue로 데이터를 넘길 수가 없는 상황을
delegate로 해결이 가능합니다.

> notification으로도 가능하나 보통 notification은 수신받는 객체가 많을때 사용되며, delegate는 하나의 객체가 여러가지 요구를 받을 때 사용됩니다.


### delegate 타입을 선언할 때 weak var 로 해야하는 이유는?

delegate 이 역시 하나의 객체이기 때문에 retain 현상이 일어날 수 있습니다. 
weak로 선언해주는 이유는 delegate가 retain 되기 때문에 메모리 누수를 막기 위해서입니다.

```swift
protocol FormClassDelegate {
    func doSomething ()
}

class FirstViewController: UIViewController, SendProtocol {
  
  weak var delegate: FormClassDelegate?
  
  func action() {
        delegate?.doSomething()
  }
}
  
class SecondViewController: UIViewController, FormClassDelegate {
  
  var firstVC: FirstViewController()
  
  override func viewDidLoad() {
    super.viewDidLoad()
      firstVC = FirstViewController()
      firstVC?.delegate = self
  }
  
  func doSomething() {
  
  }
}

// 또다른 ViewController에 SecondViewController를 객체로 만들어준다면?
class ThirdViewController: UIViewController {
  
  var secondVC: SecondViewController()
  
  override func viewDidLoad() {
    super.viewDidLoad()
      secondVC = SecondViewController()
  }
}
```
- FirstViewController의 delegate는 SecondViewController를 참조하고,
SecondViewController의 firstVC는 FirstViewController를 참조하게 됩니다.
또 ThirdViewController에서는 secondVC를 객체로 만들어주게되어 SecondViewController를 참조하고 있게 됩니다.
만약 delegate를 weak 로 선언하지 않았을 경우 ThirdViewController의 secondVC가 nil이 되었을 경우 Strong으로 선언되어 객체의 연결은 해제되지 않은채 영원히 남아버리게 되고 이는 메모리 누수로 이어집니다.

> 그렇기때문에 weak var delgate: FormClassDelegate? 로 선언해주어 메모리 누수가 발생하지 않게 방지해줘야합니다.
