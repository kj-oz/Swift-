# raywenderlich.com Swift スタイルガイド（工事中）

このスタイルガイドは他のスタイルガイドとは異なり、印刷物及びWeb上での読みやすさに重点をおいています。私達の書籍やチュートリアル、入門キットは、別々な大勢の作者によって作られていますが、その中のソースコードを、見やすくまた統一感のあるものにするためにこのスタイルガイドを作成しました。

このガイドの最終目標は、一貫性、読みやすさ、そしてシンプルであることです。

Objcetive-Cをお使いですか？それなら[Objective-C スタイルガイド](https://github.com/raywenderlich/objective-c-style-guide)も御覧ください。

## 目次

* [命名規則](#命名規則)
  * [クラス名のプレフィックス](#クラス名のプレフィックス)
* [スペースの使い方](#スペースの使い方)
* [コメント](#コメント)
* [クラスと構造体](#クラスと構造体)
  * [Selfの使用](#Selfの使用)
  * [Protocolの実装](#Protocolの実装)
* [関数の宣言](#関数の宣言)
* [クロージャの書き方](#クロージャの書き方)
* [型](#型)
  * [定数](#定数)
  * [オプショナル型](#オプショナル型)
  * [構造体の初期化](#構造体の初期化)
  * [型推定](#型推定)
  * [シンタックスシュガー](#シンタックスシュガー)
* [制御文](#制御文)
* [セミコロンの使用](#セミコロンの使用)
* [言語](#言語)
* [顔文字](#顔文字)
* [関係者一覧](#関係者一覧)


## 命名規則

クラス名、メソッド名、変数名などには、内容がよく分かる名前を、キャメルケース記述した名前を付けましょう。クラス名とモジュールレベルの定数名は先頭を大文字に、メソッド名や変数名は先頭を小文字とします。

**好ましい例：**

```swift
let MaximumWidgetCount = 100

class WidgetContainer {
  var widgetButton: UIButton
  let widgetHeightPercentage = 0.85
}
```

**好ましくない例：**

```swift
let MAX_WIDGET_COUNT = 100

class app_widgetContainer {
  var wBut: UIButton
  let wHeightPct = 0.85
}
```

関数とinitメソッドの場合、用途が完全に明確な場合以外は、すべての引数にわかりやすい名前を付けましょう。もしも関数呼び出しがより読みやすくなるのであれば、引数に外部名を付けてください。

```swift
func dateFromString(dateString: NSString) -> NSDate
func convertPointAt(#column: Int, #row: Int) -> CGPoint
func timedAction(#delay: NSTimeInterval, perform action: SKAction) -> SKAction!

// 上の関数はこんなふうに呼び出されます:
dateFromString("2014-03-14")
convertPointAt(column: 42, row: 13)
timedAction(delay: 1.0, perform: someOtherAction)
```

メソッド名の場合には、Appleの標準的な用法に倣って、最初の引数に関する説明はメソッド名に含めます。
```swift
class Guideline {
  func combineWithString(incoming: String, options: Dictionary?) { ... }
  func upvoteBy(amount: Int) { ... }
}
```

関数名を文書（チュートリアルや本の本文、ソースのコメント等）の中で参照する場合には、呼び出しの際に必要な引数名も含めて書くようにします。もしも文書の流れが明確で、関数の正確なシグネチャが大切ではない場合は、メソッド名だけで済ませても構いません。

> `convertPointAt(column:row:)` を作成した`init` の中で呼び出してください。
>
> もし `timedAction`を実装した場合には、忘れずに適切な遅延時間を設定してください。
>
> 直接 data source の `tableView(_:cellForRowAtIndexPath:)` メソッドを呼び出さないでください。

疑わしい場合には、Xcodeのジャンプバーの中をクリックすることで表示できるメソッド名の一覧を御覧ください。ここで述べているスタイルは、この一覧で表示される形式と合致しています。

![Methods in Xcode jump bar](screens/xcode-jump-bar.png)

### クラス名のプレフィックス

Swift の型は、すべて自動的にそれらの型が宣言されたモジュールの名前空間に属します。従って、名前の衝突を避けるためのプレフィックス付けは必要ありません。もし2つの異なるモジュールで同じ名前が公開されていた場合には、型の名前の前にモジュール名を付けることでその2つの型を区別することが可能です。

```swift
import MyModule

var myClass = MyModule.MyClass()
```

ですので、Swiftの型にはプレフィックスは**付けないように**してください。

もしもSwiftの型をObjective-Cのソースの中で使用するために公開する場合には、以下のような形で、適切なプレフィックス（私達の[Objective-C スタイルガイド](https://github.com/raywenderlich/objective-c-style-guide)をご覧ください）を付けてください。

```swift
@objc (RWTChicken) class Chicken {
   ...
}
```


## スペースの使い方

* スペースの節約と不要な改行を避けるために、インデントにはタブではなく、2つの半角スペースを使用してください。これは、下図のような形でXcodeの環境設定で設定しておいてください。

  ![Xcode indent settings](screens/indentation.png)

* メソッドや他の構文（`if`/`else`/`switch`/`while` 等）の中括弧は、常にその元の文と同じ行で始めて、新規の行で終了してください。
* ヒント：対象のコードを選択して（あるいは⌘A で全選択をして）から Control-I （またはメニューの Editor\Structure\Re-Indent）を実行すると、コードを整形することができます。Xcodeのテンプレートコードの中には4カラムのタブが埋め込まれているものもありますが、こうすることで簡単にそれを修正することができます。 

**好ましい例：**
```swift
if user.isHappy {
  //Do something
} else {
  //Do something else
}
```

**好ましくない例**
```swift
if user.isHappy
{
    //Do something
}
else {
    //Do something else
}
```

* 見た目の構造をわかりやくするために、メソッドとメソッドの間には1行だけ空白行を設けます。メソッド内の空白行は、機能的な分割点を示すために使うべきですが、１つのメソッドの中が多くの部分にわかれている場合には、いくつかのメソッドに分割した方が良いことが多いです。

## コメント

必要に応じて、**なぜ**	その部分のコードがその処理をしているのかをコメントしてください。コメントは常にコードの修正に合わせて修正するか、場合によっては削除してください。

本来、ソースコード本体をできるだけ読んでわかる様にすべきで、コードの中でブロックコメントに何かを書くことはさけてください。*これはリファレンス生成用のコメントには当てはまりません。*


## クラスと構造体

### どちらを使うべきか

構造体は、[値を表すもの](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_144)です。構造体は特定の個体の認識が必要のないものに使用してください。 [a, b, c] の要素からなる一つの配列は、別な、やはり [a, b, c] の要素からなる配列と同じもので、交換しても全く差し支えありません。2つの配列の表している物が全く同じなので、最初の配列を使おうが、2つ目の配列を使おうが、全く問題なのです。これが、配列が構造体に属する理由です。

クラスは、[個体を参照するもの](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_145)です。クラスは特定の個体の認識が可能なもの、あるいは固有のライフサイクルを有するものに対して使用してください。2つの「人間」のオブジェクトは2つの異なる事物を表していますので「人間」にはクラスを使います。二人の名前も誕生日も同じだったとしてもそれで二人が同じ「人間」にはなりません。しかし、1950年3月3日という日付は、別な1950年3月3日の日付オブジェクトと同じものですので、人間の「誕生日」は構造体になるでしょう。個々の日付オブジェクト自体には固有の識別は必要ありません。

Sometimes, things should be structs but need to conform to `AnyObject` or are historically modeled as classes already (`NSDate`, `NSSet`). Try to follow these guidelines as closely as possible.

### Example definition

Here's an example of a well-styled class definition:

```swift
class Circle: Shape {
  var x: Int, y: Int
  var radius: Double
  var diameter: Double {
    get {
      return radius * 2
    }
    set {
      radius = newValue / 2
    }
  }

  init(x: Int, y: Int, radius: Double) {
    self.x = x
    self.y = y
    self.radius = radius
  }

  convenience init(x: Int, y: Int, diameter: Double) {
    self.init(x: x, y: y, radius: diameter / 2)
  }

  func describe() -> String {
    return "I am a circle at \(centerString()) with an area of \(computeArea())"
  }

  override func computeArea() -> Double {
    return M_PI * radius * radius
  }

  private func centerString() -> String {
    return "(\(x),\(y))"
  }
}
```

The example above demonstrates the following style guidelines:

 + Specify types for properties, variables, constants, argument declarations and other statements with a space after the colon but not before, e.g. `x: Int`, and `Circle: Shape`.
 + Define multiple variables and structures on a single line if they share a common purpose / context.
 + Indent getter and setter definitions and property observers.
 + Don't add modifiers such as `internal` when they're already the default. Similarly, don't repeat the access modifier when overriding a method.


### Use of Self

For conciseness, avoid using `self` since Swift does not require it to access an object's properties or invoke its methods.

Use `self` when required to differentiate between property names and arguments in initializers, and when referencing properties in closure expressions (as required by the compiler):

```swift
class BoardLocation {
  let row: Int, column: Int

  init(row: Int,column: Int) {
    self.row = row
    self.column = column
    
    let closure = { () -> () in
      println(self.row)
    }
  }
}
```

### Protocol Conformance

When adding protocol conformance to a class, prefer adding a separate class extension for the protocol methods. This keeps the related methods grouped together with the protocol and can simplify instructions to add a protocol to a class with its associated methods.

Also, don't forget the `// MARK` comment to keep things well-organized!

**Preferred:**
```swift
class MyViewcontroller: UIViewController {
  // class stuff here
}

// MARK: - UITableViewDataSource
extension MyViewcontroller: UITableViewDataSource {
  // table view data source methods
}

// MARK: - UIScrollViewDelegate
extension MyViewcontroller: UIScrollViewDelegate {
  // scroll view delegate methods
}
```

**Not Preferred:**
```swift
class MyViewcontroller: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
  // all methods
}
```


## Function Declarations

Keep short function declarations on one line including the opening brace:

```swift
func reticulateSplines(spline: [Double]) -> Bool {
  // reticulate code goes here
}
```

For functions with long signatures, add line breaks at appropriate points and add an extra indent on subsequent lines:

```swift
func reticulateSplines(spline: [Double], adjustmentFactor: Double,
    translateConstant: Int, comment: String) -> Bool {
  // reticulate code goes here
}
```


## Closure Expressions

Use trailing closure syntax wherever possible. In all cases, give the closure parameters descriptive names:

```swift
return SKAction.customActionWithDuration(effect.duration) { node, elapsedTime in 
  // more code goes here
}
```

For single-expression closures where the context is clear, use implicit returns:

```swift
attendeeList.sort { a, b in
  a > b
}
```


## Types

Always use Swift's native types when available. Swift offers bridging to Objective-C so you can still use the full set of methods as needed.

**Preferred:**
```swift
let width = 120.0                                    //Double
let widthString = (width as NSNumber).stringValue    //String
```

**Not Preferred:**
```swift
let width: NSNumber = 120.0                                 //NSNumber
let widthString: NSString = width.stringValue               //NSString
```

In Sprite Kit code, use `CGFloat` if it makes the code more succinct by avoiding too many conversions.

### Constants

Constants are defined using the `let` keyword, and variables with the `var` keyword. Any value that **is** a constant **must** be defined appropriately, using the `let` keyword. As a result, you will likely find yourself using `let` far more than `var`.

**Tip:** One technique that might help meet this standard is to define everything as a constant and only change it to a variable when the compiler complains!

### Optionals

Declare variables and function return types as optional with `?` where a nil value is acceptable.

Use implicitly unwrapped types declared with `!` only for instance variables that you know will be initialized later before use, such as subviews that will be set up in `viewDidLoad`.

When accessing an optional value, use optional chaining if the value is only accessed once or if there are many optionals in the chain:

```swift
self.textContainer?.textLabel?.setNeedsDisplay()
```

Use optional binding when it's more convenient to unwrap once and perform multiple operations:

```swift
if let textContainer = self.textContainer {
  // do many things with textContainer
}
```

When naming optional variables and properties, avoid naming them like `optionalString` or `maybeView` since their optional-ness is already in the type declaration.

For optional binding, shadow the original name when appropriate rather than using names like `unwrappedView` or `actualLabel`.

**Preferred:**
```swift
var subview: UIView?

// later on...
if let subview = subview {
  // do something with unwrapped subview
}
```

**Not Preferred:**
```swift
var optionalSubview: UIView?

if let unwrappedSubview = optionalSubview {
  // do something with unwrappedSubview
}
```

### Struct Initializers

Use the native Swift struct initializers rather than the legacy CGGeometry constructors.

**Preferred:**
```swift
let bounds = CGRect(x: 40, y: 20, width: 120, height: 80)
var centerPoint = CGPoint(x: 96, y: 42)
```

**Not Preferred:**
```swift
let bounds = CGRectMake(40, 20, 120, 80)
var centerPoint = CGPointMake(96, 42)
```

Prefer the struct-scope constants `CGRect.infiniteRect`, `CGRect.nullRect`, etc. over global constants `CGRectInfinite`, `CGRectNull`, etc. For existing variables, you can use the shorter `.zeroRect`.

### Type Inference

The Swift compiler is able to infer the type of variables and constants. You can provide an explicit type via a type alias (which is indicated by the type after the colon), but in the majority of cases this is not necessary.

Prefer compact code and let the compiler infer the type for a constant or variable.

**Preferred:**
```swift
let message = "Click the button"
var currentBounds = computeViewBounds()
```

**Not Preferred:**
```swift
let message: String = "Click the button"
var currentBounds: CGRect = computeViewBounds()
```

**NOTE**: Following this guideline means picking descriptive names is even more important than before.


### Syntactic Sugar

Prefer the shortcut versions of type declarations over the full generics syntax.

**Preferred:**
```swift
var deviceModels: [String]
var employees: [Int: String]
var faxNumber: Int?
```

**Not Preferred:**
```swift
var deviceModels: Array<String>
var employees: Dictionary<Int, String>
var faxNumber: Optional<Int>
```



## Control Flow

Prefer the `for-in` style of `for` loop over the `for-condition-increment` style.

**Preferred:**
```swift
for _ in 0..<3 {
  println("Hello three times")
}

for (index, person) in enumerate(attendeeList) {
  println("\(person) is at position #\(index)")
}
```

**Not Preferred:**
```swift
for var i = 0; i < 3; i++ {
  println("Hello three times")
}

for var i = 0; i < attendeeList.count; i++ {
  let person = attendeeList[i]
  println("\(person) is at position #\(i)")
}
```


## Semicolons

Swift does not require a semicolon after each statement in your code. They are only required if you wish to combine multiple statements on a single line.

Do not write multiple statements on a single line separated with semicolons.

The only exception to this rule is the `for-conditional-increment` construct, which requires semicolons. However, alternative `for-in` constructs should be used where possible.

**Preferred:**
```swift
var swift = "not a scripting language"
```

**Not Preferred:**
```swift
var swift = "not a scripting language";
```

**NOTE**: Swift is very different to JavaScript, where omitting semicolons is [generally considered unsafe](http://stackoverflow.com/questions/444080/do-you-recommend-using-semicolons-after-every-statement-in-javascript)

## Language

Use US English spelling to match Apple's API.

**Preferred:**
```swift
var color = "red"
```

**Not Preferred:**
```swift
var colour = "red"
```

## Smiley Face

Smiley faces are a very prominent style feature of the raywenderlich.com site! It is very important to have the correct smile signifying the immense amount of happiness and excitement for the coding topic. The closing square bracket `]` is used because it represents the largest smile able to be captured using ASCII art. A closing parenthesis `)` creates a half-hearted smile, and thus is not preferred.

**Preferred:**
```
:]
```

**Not Preferred:**
```
:)
```  


## Credits

This style guide is a collaborative effort from the most stylish raywenderlich.com team members: 

* [Soheil Moayedi Azarpour](https://github.com/moayes)
* [Scott Berrevoets](https://github.com/Scott90)
* [Eric Cerney](https://github.com/ecerney)
* [Sam Davies](https://github.com/sammyd)
* [Evan Dekhayser](https://github.com/edekhayser)
* [Jean-Pierre Distler](https://github.com/pdistler)
* [Colin Eberhardt](https://github.com/ColinEberhardt)
* [Greg Heo](https://github.com/gregheo)
* [Matthijs Hollemans](https://github.com/hollance)
* [Erik Kerber](https://github.com/eskerber)
* [Christopher LaPollo](https://github.com/elephantronic)
* [Ben Morrow](https://github.com/benmorrow)
* [Andy Pereira](https://github.com/macandyp)
* [Ryan Nystrom](https://github.com/rnystrom)
* [Cesare Rocchi](https://github.com/funkyboy)
* [Ellen Shapiro](https://github.com/designatednerd)
* [Marin Todorov](https://github.com/icanzilb)
* [Chris Wagner](https://github.com/cwagdev)
* [Ray Wenderlich](https://github.com/rwenderlich)
* [Jack Wu](https://github.com/jackwu95)

Hat tip to [Nicholas Waynik](https://github.com/ndubbs) and the [Objective-C Style Guide](https://github.com/raywenderlich/objective-c-style-guide) team!

We also drew inspiration from Apple’s reference material on Swift:

* [The Swift Programming Language](https://developer.apple.com/library/prerelease/ios/documentation/swift/conceptual/swift_programming_language/index.html)
* [Using Swift with Cocoa and Objective-C](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/BuildingCocoaApps/index.html)
* [Swift Standard Library Reference](https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/SwiftStandardLibraryReference/index.html)
