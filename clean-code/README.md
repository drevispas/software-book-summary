## 2. Meaningful Names
Clean Code (by Robert C. Martin)에서 정리한 내용입니다.

### Use Intention-Revealing Names
- 이름을 설명하는데 주석이 필요하다면 의도에 맞게 이름을 짓지 못한 것이다.
  ```kotlin
  val d = 10 // distance
  ```
- Magic number 대신에 의도를 나타내는 함수를 사용한다.
  ```kotlin
  if (cell.status == FLAGGED)
  // -->
  if (cell.isFlagged())
  ```
  
### Avoid Disinformation
- Disinformation: 의도를 오해하게 만드는 단서
- 자신이 만든 약어가 일반적인 사용되는 다른 단어와 겹치면 사용하지 않는다.
  ```kotlin
  val hp = 100 // horse power의 약어로 만들었지만 다른 사람들은 컴퓨터 회사 이름으로 생각한다.
  ```
- 여러 객체 무리를 나타내기 위해서 ~List를 붙일 때 주의한다. 언어에서 제공하는 List 형이 아니라면 읽는 사람이 그런 자료구조로 착각할 수 있다. ~Group이나 복수형을 사용한다.
  ```kotlin
  val accountList = setOf(...)
  // -->
  val accounts = setOf(...) // or
  val accountSet = setOf(...)
  ```
- [ ] 두 개의 구현체를 `XYZControllerForEfficientHandlingOfStrings`, `XYZControllerForEfficientStorageOfStrings`로 이름 짓는다면 단점은? 장점은?

### Make Meaningful Distinctions

- 의미가 같은 객체 여러 개가 필요한 경우라 하더라도 이름 끝에 숫자로 구분하는 것은 의도 전달에 좋지 않다.
  ```kotlin
  arr2[i] = arr1[i]
  // -->
  destination[i] = source[i]
  ```
- 다음과 같이 noise word를 붙이는 것은 의도를 전달하는 데 의미가 없다.
  - ~Info, ~Data 접미사로 구분하기
    ```kotlin
    class ProductInfo // X
    class ProductData // X
    class ProductObject // X
    class TheProduct // X
    ```
  - 변수 이름에 "variable" 포함시키기, 테이블 이름에 "table" 포함시키기
  - 변수 이름에 데이터타입 포함시키기
    ```kotlin
    val nameString: String = "yong" // name은 이미 문자열이라는 사실을 포함하고 있다.
    ```

### Use Pronounceable Names
- 발음할 수 없는 이름은 만들지 말자.
  ```kotlin
  val genYYYYMMDD
  val modTmsp
  // -->
  val generationDate
  val modificatonTimestamp
  ```

### Use Searchable Names
- IDE 등으로 검색하기 쉬운 이름으로 짓는다.
- 숫자는 상수명으로 대체
```kotlin
val requiredClassrooms = 103 / 7 // 7로는 원하는 학급수만 검색하기 어렵다. 
val requiredChalks = 7 * 30
// -->
const val NUMBER_OF_CLASSES = 7
const val NUMBER_OF_STUDENTS = 103
const val CHALKS_PER_CLASSROOM = 30
val requiredClassrooms = NUMBER_OF_STUDENTS / NUMBER_OF_CLASSES
val requiredChalks = calculateChalks(NUMBER_OF_CLASSES, CHALKS_PER_CLASSROOM)
```
- 한 글자짜리는 작은 로컬 스코프에서만 사용

### Avoid Encodings
- 요즘 편집툴은 좋아서 Hungarian notation을 쓸 필요가 없다.
  ```kotlin
  val phoneNumberString = "010-1234-5678" // or
  val strPhoneNumber = "010-1234-5678"
  // -->
  val phoneNumber = "010-1234-5678"
  ```
- 멤버변수에 "m_" 같은 prefix를 붙일 필요가 없다. 요즘 편집툴은 멤버를 클릭하면 컬러로 멤버 변수인지 표시해준다.
  ```kotlin
  class Part(val m_desc: String)
  // -->
  class Part(val description: String)
  ```
- 인터페이스와 그 구현 클래스를 구분하기 위해 인코딩을 해야한다면 클래스 쪽을 선호한다.
  ```kotlin
  interface ShapeFactory // IShapeFactory보다는
  class ShapeFactoryImp : ShapeFactory // 구현클래스쪽에 인코딩 단어 추가
  ```

### Avoid Mental Mapping
- 도메인과 관계없는 이름을 지으면 변수명과 실제 개념 간에 맵핑을 기억해야 하는 문제가 생긴다.
- smart programmer와 professional programmer의 차이
  - 스마트 프로그래머는 mental juggling 능력을 보여줘서 똑똑함을 드러내려고 하고
  - 프로페셔널 프로그래머는 다른 사람들이 이해하기 쉽게 작성하는데 힘을 쓴다.

### Class Names
- 명사여야 한다. 동사는 안 된다.
- ~Info, ~Manager, ~Data, ~Processor 등을 포함시키지 않는다.

### Method Names
- 동사나 동사구여야 한다.
- 생성자 오버로드할 때 정적 생성 함수도 추가해주는 것이 읽기에 좋다.
```kotlin
val point = Complex(23.0)
// -->
val point = Complex.FromRealNumber(23.0) // 원래 생성자 Complext(RealNumber)는 private으로
```

### Don't Be Cute
- 이름으로 joke나 slang을 사용하지 말자.

### Pick One Word per Concept
- 여러 클래스에 걸처 함수들에 get, fetch, retrieve 등 다양하게 사용하지 말고 통일하자.
- 여러 클래스에 걸쳐 ~Controller, ~Manager, ~Handler, ~Driver를 혼용하지 말자. 예를 들어 ~Manager로 통일

### Don't Pun
- 한 용어를 여러 의미로 사용하면 안 된다.
  - 예를 들어 기존 모든 클래스에서 add~()를 두 개의 객체를 입력으로 받아 새로운 객체를 생성하는 용도로 사용하고 있었다고 하자. 그런데 이번에 만드는 새로운 클래스 함수는 한 개의 입력만 받지만 현재 객체에 새로운 아이템을 추가하는 용도이기 때문에 add를 붙이기로 하였다.
  - 위와 같은 상황이 기피해야 하는 동음이의어 상황이다. 대신 append나 insert를 사용하는 게 낫다.

### Use Solution Domain Names
- 의미가 통하는 기술적 용어(CS 용어)가 있다면 굳이 비지니스 용어(Problem domain name)을 사용하지 않아도 된다.
- 예를 들어 JobQueue는 프로그래머 모두가 이해가능한 이름이므로 굳이 도메인 전문가에게 물어서 비지니스에서 쓰이는 용어를 찾지 않아도 된다.

### Use Problem Domain Names
- 의미에 맞는 기술적 용어가 없을 경우에는 비즈니스 용어를 사용한다.
- Problem domain에 가까운 코드일수록 비즈니스 용어를 많이 사용하게 될 것이다.

### Add Meaningful Context
- 이름은 context 안에서 의미가 정확해진다.
- 함수 안에 street, city, state 같은 변수들이 섞여 있을 때 각각 주소의 부분임을 알 수 있지만 state 단독으로 나타난다면 짐작하기가 어렵다.
- addrState 같이 prefix를 붙일 수도 있지만 Address 클래스를 만들고 그 멤버로 편입시키는 것이 더 나은 context를 줄 수 있다.

### Don't Add Gratuitous Context
- 애플리케이션 이름을 접두어 모든 클래스에 붙이는 것은 불필요하다. 사실 IDE로 검색할 때 너무 많은 리스트가 나와서 불편해진다.
  ```kotlin
  class TransactionPGAddress
  class TransactionHttpHandler
  // -->
  class PGAddress
  class HttpHandler
  ```
