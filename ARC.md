
(https://docs.swift.org/swift-book/documentation/the-swift-programming-language/automaticreferencecounting/)

### ARC란
- Automatic Reference Counting(자동 참조 계산)으로 앱의 메모리 사용량을 추적하고 관리합니다.
ARC는 해당 인스턴스가 더이상 필요하지 않을때 클래스 인스턴스에서 사용하는 메모리를 자동으로 해제합니다.

참조 카운팅은 클래스 인스턴스에서만 적용됩니다. 구조 및 열거형은 참조 유형이 아닌 값 유형이며, 참조로 저장 및 전달되지 않기때문에 ARC로 관리할 필요가 없습니다.

```swift
class Person {
    let name: String
    init(name: String) {
        self.name = name
        print("\(name) is being initialized")
    }
    deinit {
        print("\(name) is being deinitialized")
    }
}

// nil값을 자동으로 초기화하여 Person 인스턴스를 참조하지 않은 상태
var person1: Person?
var person2: Person?
var person3: Person?
 
// 인스턴스 참조
person1 = Person(name: "John Appleseed")
// John Appleseed is being initialized
// 인스턴스의 참조 횟수 : 1

reference2 = reference1 // 인스턴스의 참조 횟수 : 2
reference3 = reference1 // 인스턴스의 참조 횟수 : 3

reference3 = nil    // 인스턴스의 참조 횟수 : 2
reference2 = nil    // 인스턴스의 참조 횟수 : 1
reference1 = nil    // 인스턴스의 참조 횟수 : 0
// John Appleseed is being deinitialized
```
- Person 클래스의 인스턴스가 초기화 될때 init 함수가 호출되어 "initialized" 를 출력하고,
인스턴스가 해제될 때 "deinitialized"를 출력하는 소멸자 함수를 가지고 있습니다.

- person1 변수에 Person 인스턴스가 할당되면서 강한 참조를 형성하게 됩니다.(인스턴스 참조 횟수 : 1)
참조횟수가 0이 되는 순간 인스턴스는 ARC 규칙에 의해 메모리에서 해제되며, 메모리에서 해제되기 직전에 deinitializer를 호출합니다.

----------

### Strong Reference Cycles Between Class Instances
(클래스 인스턴스 간 강한 참조)

- 인스턴스간 서로를 강하게 참조할 때 문제가 발생할 수 있습니다.

```swift
class Person {
    let name: String
    init(name: String) { 
        self.name = name 
    }
    var room: Room? 
    deinit { 
        print("\(name) is being deinitialized") 
    }
}


class Room {
    let number: String
    init(number: String) { 
        self.number = number 

    }
    var host: Person?
    deinit { 
        print("Room \(number) is being deinitialized") 
    }
}
```

```swift
var siyeon: Person? = Person(name: "siyeon")  // Person 인스턴스의 참조 횟수 : 1
var room: Room? = Room(number: "1004")   // Room 인스턴스의 참조 횟수 : 1
```
- 변수 siyeon과 room은 강한 참조를 사용하여 서로를 참조하고 있다.
변수 siyeon은 Person 인스턴스에 대한 강한 참조를 가지고 있고, 변수 room은 Room 인스턴스에 대한 강한 참조를 가지고 있다.

```swift
room?.host = siyeon  // Person 인스턴스의 참조 횟수 : 2
siyeon?.room = room  // Room 인스턴스의 참조 횟수 : 2
```
- room과 siyeon 변수 내에  해당 인스턴스의 프로퍼티를 설정하여 두 인스턴스를 연결하였습니다.
Person 인스턴스는 Room 인스턴스에 대해 강한 참조를 갖게 되고, Room 인스턴스는 Person 인스턴스에 대해 강한 참조를 갖게 됩니다.

```swift
siyeon = nil // Person 인스턴스의 참조 횟수 : 1
room = nil  // Room 인스턴스의 참조 횟수 : 1

// Person 인스턴스를 참조할 방법 상실 - 메모리에 잔존
// Room 인스턴스를 참조할 방법 상실 - 메모리에 잔존
```
- siyeon, room 변수를 각각 nil로 설정할 때 deinit 구문이 호출되지 않습니다. 강한 참조 사이클은 Person과 Room 인스턴스가 할당 해제되는것을 방지하여 앱에서 메모리 누수를 유발합니다.
따라서 변수가 강한 참조를 중단해도 참조 횟수는 0이 되지 않으며, Person 과 Room 인스턴스 간의 강한 참조는 남아있고 중단될 수 없습니다.
