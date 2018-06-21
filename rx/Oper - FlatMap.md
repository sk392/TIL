##Oper - flatMap

Rx에서 Observable에서 발행한 아이템을 다른 Observable로 만들며, 만들어진 Observable에서 아이템을 발행합니다.

####flatMap


RxSwift에서 제공하는 예제를 살펴보면 좀 더 쉽게 이해할 수 있습니다.

```

let sequenceInt = Observable.of(1, 2, 3)	// Int 타입 시퀀스
let sequenceString = Observable.of("A", "B", "C", "D")	// String 타입 시퀀스

sequenceInt
	.flatMap { (x: Int) -> Observable<String> in
		print("Emit Int Item : \(x)")
		return sequenceString
	}
	.subscribeNext {
		print("Emit String Item : \($0)")
}

// Output
Emit Int Item : 1
Emit String Item : A
Emit String Item : B
Emit String Item : C
Emit String Item : D
Emit Int Item : 2
Emit String Item : A
Emit String Item : B
Emit String Item : C
Emit String Item : D
Emit Int Item : 3
Emit String Item : A
Emit String Item : B
Emit String Item : C
Emit String Item : D

```

위의 코드에서 sequenceInt는 Int 아이템을 발생을 하며, flatMap을 통해 새로운 String 타입 Observable 시퀀스를 반환합니다.

즉, sequenceInt에서 발행한 아이템에서 새로운 Observable을 만들고, 발행한 아이템을 구독하여 출력합니다.


그렇다면 flatMap으로 비동기 Observable을 반환하면 어떻게 될까요?

다음은 타이머 Observable을 만드는 코드입니다.
```
	let t = Observable<Int>
		.interval(0.5, scheduler: MainScheduler.instance)	// 0.5초마다 발행
		.take(4)		// 4번 발행

	t.flatMap { (x: Int) -> Observable<Int> in
		let newTimer = Observable<Int>
			.interval(0.2, scheduler: MainScheduler.instance)	// 0.2초마다 발행
			.take(4)		// 4번 발행
			.map { _ in x }		// 전달받은 아이템을 그대로 전달
		return newTimer
		}
		.subscribe {
			print("Result : \($0)")
	}

	// Output
	Result : Next(0)
	Result : Next(0)
	Result : Next(0)
	Result : Next(1)
	Result : Next(0)
	Result : Next(1)
	Result : Next(1)
	Result : Next(2)
	Result : Next(1)
	Result : Next(2)
	Result : Next(2)
	Result : Next(3)
	Result : Next(2)
	Result : Next(3)
	Result : Next(3)
	Result : Next(3)
	Result : Completed


위의 코드가 실행되면, flatMap으로 만들어진 타이머 Observable로 인해 0.5초 간격으로 여러 개의 Observable이 동시에 수행하게 됩니다.


위 코드 실행 결과로 다음과 같은 스트림 형태를 나타낼 수 있습니다.



	t    : ----------0---------1----------2----------3X
	new0 :           ---0---0---0---0x
	new1 :                     ---1---1---1---1x
	new2 :                                ---2---2---2---2x
	new3 :                                           ---3---3---3---3X
	subs : -------------0---0---0-1-0-1---1--21--2---2--32--3---3---3x
