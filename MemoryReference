
### Strong

- 해당 인스턴스에 대한 소유권 가짐
- 자신이 참조하는 인스턴스의 retain count 증가시킴
- 값 지정 시점에 retain, 참조 종료 시점에 release
- 선언 시 특별한 지정 없으면 → strong이 default

```swift
	var test = Test()   // retain count 1증가
	test = nil   // retain count 1 감소 -> 0이 되면서 release (메모리 해제)
```

- 메모리 누수가 발생할 수 있음 → 약한 참조 (weak, unknown)로 해결책
- IBOutlet 에서 Strong 사용시 : 뷰가 복잡한 계층 구조로

> Weak 와 Unowned : **Reference count 를 증가시키지 않음**
> 

### Weak

- 해당 인스턴스에 대한 소유권 가지지 않음, 주소값만 가짐
- 자신이 참조하는 인스턴스의 retain count 증가 X, release도 발생 X
- 해당 객체가 nil이 될 수 있는 optional 타입
- 대표적인 예로는 delegate 패턴이 있다.

```swift
weak var test = Test()   // 객체가 생성되지만 weak 이기 때문에 바로 객체가 해제되어 nil이 됨
```

### Unowned

- weak과 같은 약한 참조 이지만, 해당 객체는 절대 nil 일 수 없음 → Non Optional 타입으로 선언되어야 한다.
- 매우 위험한 방법
- 메모리에서 해제되지 않을 때에만 사용해야 한다. 만약 객체에 엑세스할 경우 런타임 오류를 발생시킨다.

> weak 와 unknown은 순환 참조로 인한 메모리 누수가 예상될 대 합리적인 코딩을 통해 해결할 수 있음
