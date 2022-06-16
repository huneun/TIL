# 코틀린의 Smart Cast와 자바의 instanceOf의 차이

### Smart Cast는 기능 자체를 부르는 말!, instanceOf는 메소드!
 Smart Cast와 instanceOf의 차이를 자꾸 놓치지 않기 위해 선행되야 할 구분. 
 Smart Cast는 타입검사와 타입 캐스트를 조합한 기능을 일컫는 말이고 instanceOf는 타입검사를 위한 메소드이다. 
 
 #### 코틀린은 is 키워드를 사용하여 타입검사 및 타입캐스팅

(예시_자바의 instanceOf)
아래 처럼 자바의 instanceOf는 타입검사만 하기 때문에 해당 타입에 대한 조건 이후에는 형변환을 명시하여 사용하는 구문이 오게 된다.
<pre>
	<code>
		Car car = new Car();

		if (car instanceof Toy) {
			Toy lego = (Toy)car; 
			lego.play(); 
		}

	</code>
</pre>


(예시_코틀린의 is 키워드)
is 키워드가 타입검사 뿐 아니라 타입캐스트도 하기 때문에 형변환된 타입의 변수를 다시 선언할 필요가 없다!
<pre>
	<code>
		val car = Car() // 여기서 car는 Car 타입
		if(car is Toy){
			car.play() // 여기서 car는 Toy 타입
		}
	</code>
</pre>

#### 스마트 캐스트를 사용하기 위한 주의사항
##### 1) 스마트 캐스트로 사용되는 값은 타입검사 이후 그 값이 바뀔 수 없는 경우에만 작동한다. 즉, 반드시 해당 프로퍼티는 val 키워드를 사용해야한다.
(이것은 클래스에도 해당하는 내용으로서 기본적으로 클래스 앞에 아무 키워드도 없다면 컴파일러는 final이 붙은 것으로 인식한다. 이는 클래스도 스마트 캐스트를 하기 위해서 값이 바뀔수 없는 경우에만 가능하게 한다는 뜻이다. 이렇게 val, final 키워드를 기본적으로 사용하는 이유는 자바에서의 '취약한 기반 클래스' 문제에 대해 해결기준을 명시한 것이다.)
##### 2) 원하는 타입으로 명시적 타입 캐스트를 하는 방법은 as 키워드를 사용한다. (예 - val n = car as Toy )


### 더 나아가면

같은 인스턴스를 구현한 두 개의 클래스에 대한 스마트 캐스트를 통해 유연하고 간결한 코드를 구성할 수 있음.

여기 Expr라는 인터페이스를 구현한 클래스 Num과 Sum이 있다. Num은 프로퍼티 value 값을 저장하고 반환하는 기능을.
Sum은 받은 프로퍼티 두개를 더하여 돌려주는 기능을 구현하기 위해 메소드 eval(e: Expr) 구문에 스마트 캐스트 기능을 이용하면 다음과 같다.
<pre>
	<code>
		interface Expr
		class Num (val value: Int) : Expr // 인터페이스 Expr를 구현한 value라는 프로퍼티 한 개를 가진 Num 클래스
		class Sum (val left: Expr, val right: Expr) : Expr // 인터페이스 Expr를 구현한 left와 right라는 프로퍼티 두 개를 가진 Sum 이란 클래스
	</code>
</pre>

<pre>
	<code>
		fun eval(e: Expr) : Int {
			if(e is Num) {
				return e.value	
			}
			
			if(e is Sum) {
				return e.left + e.right
			}
		}
	</code>
</pre>

