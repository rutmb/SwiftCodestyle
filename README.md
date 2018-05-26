# Swift Code Style

## Содержание

* [Общее](#Общее)
    * [Табуляция](#Табуляция)
    * [Операторные скобки](#Операторные-скобки)
    * [Типы данных](#Типы-данных)
    * [Разделение операторов](#Разделение-операторов)
    * [Объявление переменных](#Объявление-переменных)
* [Именование переменных](#Именование-переменных)
    * [Перечисляемые типы](#Перечисляемые-типы)
    * [Протоколы](#Протоколы)
    * [Селекторы](#Селекторы)
    * [Генерики](#Генерики)
    * [Tuples](#Tuples)
    * [Typealiases](#Typealiases)
* [Организация кода](#Организация-кода)
    * [Структура класса](#Структура-класса)
    * [Комментарии](#Комментарии)
    * [Вычислимые свойства](#Вычислимые-свойства)
    * [Замыкания](#Замыкания)
    * [Автовыведение типов](#Автовыведение-типов)
    * [Константы](#Константы)
    * [Опционалы](#Опционалы)
    * [Структуры](#Структуры)
    * [Ленивая инициализация](#Ленивая-инициализация)
    * [Синтаксический сахар](#Синтаксический-сахар)
    * [Методы и функции](#Методы-и-функции)
* [Memory management](#memory-management)
    * [Weak, retain, strong](#weak-retain-strong)
* [Golden path](#golden-path)
    * [Guard](#guard)
    * [If let](#if-let)
* [Правила хорошего тона](#Правила-хорошего-тона)

## Язык

Предполагается использование английского языка для именования переменных, методов, классов.

Далее для каждого проекта отдельно команда решает, стоит ли использовать для комментариев, commit messages и документации русский или английский. Определяется язык проекта. При плохом владении английским языком хотя бы у **одного** члена команды, необходимо всей команде использовать русский язык. Для иностранных проектов использование английского языка обязательно.
Смотрим в репозиторий, спрашиваем у других, затем пишем commit message на нужном языке. Не нужно писать на ломаном английском в проекте, где все пишут комментарии на русском, а также оставлять непонятные русские комментарии в проекте с иностранным заказчиком.

**Нежелательно:**
```swift
let zagruzkaColor = UIColor.white()
```

**Желательно:**
```swift
let loadingColor = UIColor.white()
```

## Общее

### Табуляция
Используются отступы из 4 пробелов. Настроить это можно в меню XCode - Preferences - Text Editing - Indentation.

### Операторные скобки
Принято открывающую скобку ставить на той же строке. В конструкциях с цепочками if-else-if `else` не переносится:
```swift
if a > b {
	// do stuff
} else {
	// do another
}
```

### Типы данных

Не использовать `Objective-C`-структуры данных там, где есть их явный `Swift`-аналог.

**Нежелательно:** 
```swift
class MyClass: NSObject {
    var myArray: NSArray<NSString>
    
    init() {
        myArray = NSArray()
        super.init()
    }
}
```
**Желательно:** 
```swift
class MyClass {
    var myArray: [String]
    
    init() {
        myArray = []
    }
}
```

### Разделение операторов
Точку с запятой (`;`) не ставить, использовать перенесение на новую строку для разделения операторов.

**Нежелательно:**
```swift
var a = 5; a = 10;
```

**Желательно:**
```swift
var a = 5
a = 10
```

### Объявление переменных
Использовать `let` при объявлении переменных по возможности всегда.

## Именование переменных

* Стараться не сокращать переменные, давать понятные имена, отражающие роль переменной в контексте. 
* Не использовать венгерскую нотацию (например, `kCLLocationManagerFilterNone`), snake case (`like_so`), или macro case (`LIKE_THIS`).
* Опускать ненужные слова:

**Нежелательно:** 
```swift
print(myCart.cartWeight)
print(myCar.carSpeed)
```
**Желательно:** 
```swift
print(myCart.weight)
print(myCar.speed)
```

* Называйте функции с параметрами так, чтобы было понятно, что такое первый параметр.
Если это невозможно, укажите явную метку для первого параметра.
Именование переменных должно вести к тому, чтобы вызовы методов складывались в корректные английские фразы:

**Нежелательно:** 
```swift
array.remove(x) // что такое x? Объект или индекс?

x.insert(y, position: z)
x.subViews(color: y)
x.nounCapitalize()

class Person {
  init(_ firstName: String, _ lastName: String) {
  // No external parameter names when called: Person("Some", "Guy")
}

  func set(name: String) {
  // It's not clear what the parameter is: set("Mr Bill")
  }

  func setSomething(firstName: String, lastName: String) {
  // The method name has nothing to do with the parameters: setSomething("John", lastName: "Smith")
  }
}
```
**Желательно:** 
```swift
array.remove(at: index)
array.remove(item)

x.insert(y, at: z)          // x, insert y at z
x.subViews(havingColor: y)  // x's subviews having color y
x.capitalizingNouns()       // x, capitalizing nouns

class Person {
  init(firstName: String, lastName: String) {
  // Called as Person(firstName: "...", lastName: "...")
  }

  func setName(name: String) {
  // Called as setName("...")
  }

  func setName(first firstName: String, last lastName: String) {
  // Called as setName(first: "...", last: "...")
  }
}
```

* Именование параметров должно выделять единый уровень абстракции:

**Нежелательно:** 
```swift
a.move(toX: b, y: c)
a.fade(fromRed: b, green: c, blue: d)
```
**Желательно:** 
```swift
a.moveTo(x: b, y: c)
a.fadeFrom(red: b, green: c, blue: d)
```

* Необходимо вводить дополнительные метки параметров, если это определяет предназначение:

**Нежелательно:** 
```swift
viewController.dismiss(false) 
words.split(12)
```
**Желательно:** 
```swift
viewController.dismiss(animated: false) 
words.split(maxCount: 12)
```

* Именование мутирующих и не мутирующих функций должны отражаться в их сигнатуре:

```swift
// мутирующие
array.sort()
x.append(y)

// немутирующие
let result = array.sorted()
let z = x.appending(y)
```

* Использовать где возможно значения параметров по умолчанию:

```swift
  extension String {
      public func compare(other: String, 
                        options: CompareOptions = [], 
                          range: Range? = nil, 
                         locale: Locale? = nil) -> Ordering
  }
  
  // Желательно:
  let order = lastName.compare(royalFamilyName)
  
  // Нежелательно:
  let order = lastName.compare(royalFamilyName, options: [], range: nil, locale: nil)
```

### Перечисляемые типы

Элементы перечислимого типа именовать в camelCase, начиная с маленькой буквы, если используется Swift 3 и выше версия:

```swift
// Swift 3
enum Direction {
    case up, down, left, right
}

```

### Протоколы

Имена протоколов должны быть выражены в виде имен существительных, если оно описывает **предназначение** (например, `Collection`), или добавляются суффиксы `-able`, `-ed`, `-ing`, если это описывает **поведение** (например, `Equatable`, `ProgressReporting`).

Если ваш протокол должен иметь опциональные методы, он должен быть объявлен с атрибутом `@objc`.
Объявите определения протокола рядом с классом, использующим делегат, а не с классом, реализующим методы делегата.
Если несколько классов используют один и тот же протокол, объявите его в собственном файле.
Используйте  `weak var` для переменных делегата для того чтобы избежать `retain cycle`.

```
//SomeTableCell.swift

protocol SomeTableCellDelegate: class {
  func someTableCell(cell: SomeTableCell, didTapSignUpButton button: UIButton)
}

class SomeTableCell: UITableViewCell {
  weak var delegate: SomeTableCellDelegate?
  // ...
}
//SomeTableViewController.swift

class SomeTableViewController: UITableViewController {
// ...
}

// MARK: - SomeTableCellDelegate

extension SomeTableViewController: SomeTableCellDelegate {
  func someTableCell(cell: SomeTableCell,   didTapSignUpButton button: UIButton) {
  // Implementation of cellbuttonwasTapped method
  }
}

```

### Селекторы

Использовать синтаксис вида `#selector(doSomething)`, не включая имя собственного класса (вроде `#selector(MyViewController.doSomething)`).

### Генерики

Указывать не традиционное `T` или `U` в качестве имени обобщенного параметра, а более определенные, вроде `Element` или `Item`. 

Исключениями здесь могут быть, например, пользовательские операторы, являющиеся очень высокой абстракцией над данными.

### Tuples

Создавайте имена членов кортежей `Tuples` при создании:

```
let foo = (something: "cats", somethingElse: 909_099)
let (something, somethingElse) = foo

```

### Typealiases

Создайте `typealiases`, чтобы придать семантический смысл часто используемым типы данных и замыканиям.
Typealias эквивалентно `typedef`  в C и должны использоваться для создания имен для типов.

## Организация кода

### Структура класса

Класс имеет следующую структуру:

```swift

class MyClass: BaseClass {

    // MARK: - Constants
    
    private enum Constants {
    	static let isUsingLasers: Bool = true
	static let animationTime: TimeInterval = 0.3
    }
    
    // MARK: - Properties 
    
    var citySelected: PublishSubject<City>? = nil
    var text: String { get { ... } set { ... } }
    
    // MARK: - IBOutlets
    
    @IBOutlet private weak var titleLabel: UILabel!
    
    // MARK: - NSLayoutConstraints
    
    @IBOutlet private weak var titleLabelTopConstraint: NSLayoutConstraint!
    
    // MARK: - IBActions
    
    @IBaction private func cancel(_ sender: UIButton) { ... }
    
    // MARK: - Initialization and deinitialization
    
    init?(city: anotherCity) { ... }
    
    // MARK: - BaseClass
    
    override func awakeFromNib() { ... }
    override func viewDidLoad() { ... }
    
    // MARK: - Internal methods

    func fill(city: City) { ... }
    func getPopulation() -> Int { ... }

}

// MARK: - Comparable

extension MyClass: Comparable { ... }

// MARK: - Equatable

extension MyClass: Equatable { ... }

// MARK: - Private methods

private extension MyClass { ... }
```

* Старайтесь опираться на нее и разделять блоки кода с помощью `// MARK: -` для лучшей читабельности.
* Соблюдение протоколов класса должны быть выделены в `extension`-блоки, как в примере выше.
* Все методы, используемые как вспомогательные, должны быть скрыты в приватном расширении класса, как показано выше. 
* Убираем ненужные пустые методы вроде `override func viewDidRecieveMemoryWarning() { super.didRecieveMemoryWarning() }`.

### Комментарии

В местах, где они нужны, комментарии должны объяснять, зачем некоторый код выполняет свою логику. Любые комментарии должны быть или релевантными и актуальными, или их не должно быть вовсе.

Также не стоит забывать, что комментарии в виде `///` и `/** */` подхватываются IDE и впоследствие видны в панели справа.

### Вычислимые свойства

Если свойство имеет только один аксессор, то лучше его опустить:

**Нежелательно:** 
```swift
var emptyString: String {
    get {
    	return ""
    }
}
```
**Желательно:** 
```swift
var emptyString: String {
    return ""
}
```

### Замыкания

* Пользуемся механизмом автовыведения типов: не указываем тип входных, и, если возможно, выходных параметров:

**Нежелательно:** 
```swift
[1, 2, 3, 4, 5].filter { (a: Int) -> Bool in
    return a > 3
}
```
**Желательно:** 
```swift
[1, 2, 3, 4, 5].filter { a in return a > 3 }
```

* Дополняя пример выше: не вводим именование входных параметров, если возможно, а так же используем неявный `return`:

**Еще желательнее:** 
```swift
[1, 2, 3, 4, 5].filter { $0 > 3 }
```

* Используем синтаксис trailing closures:

**Нежелательно:** 
```swift
a.perform(completion: { result in 
    // ...
})

UIView.animateWithDuration(0.3, completion: { finished in 
    // ... 
})
```
**Желательно:** 
```swift
a.perform { result in 
    // ...
}

UIView.animateWithDuration(0.3) { finished in 
    // ... 
}
```

* При использование цепочки методов, принимающих функции преобразования, код должен быть отформатирован соответствующе:

**Нежелательно:** 
```swift
["my", "gem", "deer"].map { $0.characters.count }.filter { $0 > 2 }.filter { $0 % 2 == 0 }
```
**Желательно:** 
```swift
["my", "gem", "deer"].map { $0.characters.count }
    .filter { $0 > 2 }
    .filter { $0 % 2 == 0 }
```

**NB**: Это же касается и FRP-операторов *RxSwift*.

### Автовыведение типов

Оно есть. И важно про него не забывать. Не указываем тип до тех пор, пока компилятор уж совсем к стенке не прижмет.

**Нежелательно:** 
```swift
var obvslString: String = ""
var obvslArray: [String] = [String]()
var obvslDouble: Double = 14.57
```
**Желательно:** 
```swift
var obvslString = ""
var obvslArray: [String] = []
var obvslDouble = 14.57
var notSoObviouslyFloat: CGFloat = 13.012
```

### Константы

* Локальные константы класса достаточно объявить `let`-конструкцией.
* Общепроектные константы лучше не делать глобальные, а определить в рамках `enum`:

```swift
enum Colors {
    static let customRed = UIColor(red: 250 / 255.0, green: 15 / 255.0, blue: 20 / 255.0, alpha: 1.0)
    static let customBlue = UIColor(red: 10 / 255.0, green: 0, blue: 200 / 255.0, alpha: 1.0)
}
```

### Опционалы

* Не использовать `!` при объявлении переменных! Исключений всего 2: это взаимодействие с Objective-C кодом и невозможность вычислить инициализирующее значения до достижения некоторых условий. Например, нельзя прочитать `frame` некоторой `UIView` до того, как она загружена из `xib`:

```swift
class MyView : UIView {

    // MARK: - IBOutlets

    @IBOutlet private var button : UIButton!

    // MARK: - Properties

    var buttonOriginalWidth : CGFloat!

    // MARK: - UIView

    override func awakeFromNib() {
        self.buttonOriginalWidth = self.button.frame.size.width
    }

}
```

* Не использовать `!` для force unwrapping. Вместо этого использовать `?.` или `if-let` конструкции.

```swift 
let latitude = me.homeTown?.location?.latitude
if let town = me.homeTown, location = town.location {
    // access latitude, longitude
}
```

### Структуры

При работе с CoreGraphics использовать инициализаторы структур вместо глобальных С-функций:

**Нежелательно:** 
```swift
CGPointMake(1, 1)
CGRectMake(0, 0, 100, 20)
```
**Желательно:** 
```swift
CGPoint(x: 1, y: 1)
CGRect(x: 0, y: 0, width: 100, height: 20)
```

### Ленивая инициализация

Если инициализирующий вычислимый код занимает много места, разумно выделить его в метод-фабрику с приставкой `make`. Важно отметить, что `[unowned self]` в таком случае использовать не нужно, так как не создается retain cycle.

```swift
lazy var myButton = self.makeButton()
// ...
private func makeButton() -> UIButton { ... }
```

### Синтаксический сахар

**Нежелательно:** 
```swift
let a: Array<String> = //...
```
**Желательно:** 
```swift
let a: [String] = //...
```

### Методы и функции

* В большинстве случаев, каждый новый элемент функциональности привязываем в качестве метода к существующим классам или, если метод чист, возможно вынесение в расширение протокола или существующего типа данных.

* Допускается использование глобальных функций, если они симметричны и понятны, а также если раскрывают предназначение глобального оператора. 
*Примеры*: `zip(a, b)`, `max(a,b)`, `sin(x)`.

## Memory management

### Weak, retain, strong

* Пара объектов не должна держать сильные (`strong`) ссылки друг на друга во избежание утечек памяти.
* Отличие между `weak` и `unowned` можно почерпнуть из документации Apple:

> Use a weak reference whenever it is valid for that reference to become nil at some point during its lifetime. Conversely, use an unowned reference when you know that the reference will never be nil once it has been set during initialization.

Кроме того, [этот ответ](http://stackoverflow.com/a/26025176) на StackOverflow поясняет все очень подробно.
* Если в замыкании некоторого метода происходит захват `self`, то необходимо пользоваться следующей логикой:

**Желательно:** 
```swift
resource.request().onComplete { [weak self] response in
    guard let `self` = self else { return }
    let model = self.updateModel(response)
    self.updateUI(model)
}
```
**Нежелательно:** 
```swift
// может упасть, если self выгружен из памяти до того, как получен ответ (response)
resource.request().onComplete { [unowned self] response in
  let model = self.updateModel(response)
  self.updateUI(model)
}

// возможна выгрузка self из памяти в момент между обновлением модели и обновлением UI
resource.request().onComplete { [weak self] response in
  let model = self?.updateModel(response)
  self?.updateUI(model)
}
```

## Golden path

### Guard

* Нужно использовать, чтобы сразу выйти из метода при недостижении некоторых условий:
```swift
func vendAllNamed(itemName: String) throws {
    guard isEnabled else {
        throw VendingMachineError.Disabled
    }
    
    let items = getItemsNamed(itemName)
    
    guard items.count > 0 else {
        throw VendingMachineError.OutOfStock
    }
    
    let totalPrice = items.reduce(0, combine: +)
    
    guard coinsDeposited >= totalPrice else {
        throw VendingMachineError.InsufficientFunds
    }
    
    coinsDeposited -= totalPrice
    removeFromInventory(itemName)
    dispenseSnacks(items)
}
```

* Можно использовать, чтобы избежать вложенности в `if-let`-конструкциях:
```swift
func taskFromJSONResponse(jsonData: NSData) throws -> Task {
    guard let json = decodeJSON(jsonData) as? [String: AnyObject] else {
        throw ParsingError.InvalidJSON
    }
    
    guard let id = json["id"] as? Int,
          let name = json["name"] as? String,
          let userId = json["user_id"] as? Int,
          let position = json["pos"] as? Double
    else {
        throw ParsingError.MissingData
    }
    
    return Task(id: id, name: name, userId: userId, position: position)
}
```

* Не использовать, как "обратный `if`"
* Не использовать, если в `else`-ветке содержится сложная логика.

### If let

Использование `if-let` не должно приводить к вложенности:

**Нежелательно:** 
```swift
if let my = a.myself {
    if let myDog = my.dog {
        if let dogsAge = myDog.age {
            print("My dog is \(dogsAge) years old")
        }
    }
}
```
**Желательно:** 
```swift
if let me = a.myself, 
       myDog = me.dog, 
       dogsAge = myDog.age {
    print("My dog is \(dogsAge) years old")
}
```

## Правила хорошего тона

* Если большой кортеж или сигнатура функции начинают повторяться, что разумно выделить `typealias` и использовать его:

**Нежелательно:** 
```swift
func getLocationFromIp(completion: (Address? -> Void)?) { ... }
func getLocationFromCoreLocation(completion: (Address? -> Void)?) { ... }
func getLocationWithMagic(completion: (Address? -> Void)?) { ... }
```
**Желательно:** 
```swift
typealias AddressBlock = (Address? -> Void)

func getLocationFromIp(completion: AddressBlock?) { ... }
func getLocationFromCoreLocation(completion: AddressBlock?) { ... }
func getLocationWithMagic(completion: AddressBlock?) { ... }
```

* Если в проекте используется *SwiftGen*, то желательно создать скрипт в `Build Phases`, выполняющий пересоздание соответствующих файлов.

* Для повторяющихся кусков кода на локальном уровне хорошо внедрить локальную функцию:

**Нежелательно:** 
```swift
tableView.registerNib(UINib(nibName: MapActionHeaderCell.nameOfClass, bundle: nil), forCellReuseIdentifier: MapActionHeaderCell.nameOfClass)
tableView.registerNib(UINib(nibName: ActionFilledButtonCell.nameOfClass, bundle: nil), forCellReuseIdentifier: ActionFilledButtonCell.nameOfClass)
tableView.registerNib(UINib(nibName: ActionTitledButtonCell.nameOfClass, bundle: nil), forCellReuseIdentifier: ActionTitledButtonCell.nameOfClass)
tableView.registerNib(UINib(nibName: LoadingCell.nameOfClass, bundle: nil), forCellReuseIdentifier: LoadingCell.nameOfClass)
tableView.registerNib(UINib(nibName: TextCell.nameOfClass, bundle: nil), forCellReuseIdentifier: TextCell.nameOfClass)
tableView.registerNib(UINib(nibName: AddressInfoHeaderCell.nameOfClass, bundle: nil), forCellReuseIdentifier: AddressInfoHeaderCell.nameOfClass)
tableView.registerNib(UINib(nibName: ImageCell.nameOfClass, bundle: nil), forCellReuseIdentifier: ImageCell.nameOfClass)
```
**Желательно:** 
```swift
let registerCell: UITableViewCell.Type -> Void = { type in
    self.tableView.registerNib(UINib(nibName: type.nameOfClass, bundle: nil), forCellReuseIdentifier: type.nameOfClass)
}
registerCell(MapActionHeaderCell)
registerCell(ActionFilledButtonCell)
registerCell(ActionTitledButtonCell)
registerCell(LoadingCell)
registerCell(TextCell)
registerCell(AddressInfoHeaderCell)
registerCell(ImageCell)
```
* Почему-то часто встречающийся косяк с двоеточием: точно так же, как и в русском языке, слева от него не ставится пробел, а справа ставится.

**Нежелательно:** 
```swift
let a:Int = 4
func do(x : UIImage) { ... }
```
**Желательно:** 
```swift
let a: Int = 4
func do(x: UIImage) { ... }
```
