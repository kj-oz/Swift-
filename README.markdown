**訳注：**このドキュメントは、iOS開発のチュートリアルなどで有名な[raywenderlich.com](http://www.raywenderlich.com)がGitHub上で公開している[The Official raywenderlich.com Swift Style Guide.](https://github.com/raywenderlich/swift-style-guide)を筆者が個人的に和訳し、原作者の許可をいただいた上て公表しているものです。2015/04/16現在、オリジナルの2015/04/09版(4f60c0f)を元に翻訳しています。


# raywenderlich.com Swift 公式スタイルガイド

このスタイルガイドは他のスタイルガイドとは異なり、印刷物及びWeb上での読みやすさに重点が置かれています。私達の書籍やチュートリアル、入門用サンプルは、様々な大勢の作者によって作られていますが、その中のソースコードを、見やすくまた統一感のあるものにするためにこのスタイルガイドを作成しました。

このガイドの目指すゴールは、簡潔さ、読みやすさ、そしてシンプルであることです。

Objcetive-Cをお使いですか？それなら[Objective-C スタイルガイド](https://github.com/raywenderlich/objective-c-style-guide)も御覧ください。

## 目次

* [命名規則](#命名規則)
  * [列挙型](#列挙型)
  * [文書内での表記](#文書内での表記)
  * [クラス名のプレフィックス](#クラス名のプレフィックス)
* [空白の使い方](#空白の使い方)
* [コメント](#コメント)
* [クラスと構造体](#クラスと構造体)
  * [selfの使用](#selfの使用)
  * [プロトコルへの適合](#プロトコルへの適合)
  * [計算型プロパティ](#計算型プロパティ)
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
* [クレジット](#クレジット)


## 命名規則

クラス名、メソッド名、変数名などには、キャメルケースの形式で内容のよく分かる名前を付けましょう。クラス名は先頭を大文字に、メソッド名や変数名は先頭を小文字にします。

**好ましい例：**

```swift
private let maximumWidgetCount = 100

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

関数とイニシャライザの場合、用途が完全に明確な場合以外は、すべての引数にわかりやすい名前を付けましょう。もしも関数呼び出しがより読みやすくなるのであれば、引数に外部引数名を付けてください。

```swift
func dateFromString(dateString: String) -> NSDate
func convertPointAt(#column: Int, #row: Int) -> CGPoint
func timedAction(#delay: NSTimeInterval, perform action: SKAction) -> SKAction!

//上の関数はこんなふうに呼び出されます:
dateFromString("2014-03-14")
convertPointAt(column: 42, row: 13)
timedAction(delay: 1.0, perform: someOtherAction)
```

メソッドの場合には、Appleの標準的な用法に倣って、最初の引数に関する説明はメソッド名に含めてください。

```swift
class Guideline {
  func combineWithString(incoming: String, options: Dictionary?) { ... }
  func upvoteBy(amount: Int) { ... }
}
```

### 列挙型

列挙型の値には、先頭が大文字のキャメルケースを使用してください。

```swift
enum Shape {
  case Rectangle
  case Square
  case Triangle
  case Circle
}
```

### 文書内での表記

関数名を文書（チュートリアルや書籍の本文、ソースコードのコメントなど）の中で参照する場合には、呼び出しの際に必要な引数名も含めて書くようにします。外部名が不要な引数の場合には`_`を書きます。

> 作成した`init`の中で`convertPointAt(column:row:)`を呼び出してください。
>
> もし`viewDidLoad()`の中で`timedAction(_:)`を呼び出す場合には、忘れずに調整後の遅延時間を設定してください。
>
> 直接データソースの`tableView(_:cellForRowAtIndexPath:)`メソッドを呼び出さないでください。

迷った場合には、Xcodeのジャンプバーに表示されるメソッドのリストを参考にしてください。ここで述べているスタイルは、このリストで表示される形式と合致しています。

![Methods in Xcode jump bar](screens/xcode-jump-bar.png)

### クラス名のプレフィックス

Swift の型は、すべて自動的にそれらの型が宣言されているモジュールの名前空間に属しますので、クラス名にプレフィックスをつけるべきではありません。もし2つの異なるモジュールの同じ名前が衝突する場合には、型の名前の前にモジュール名を付けることでその2つの型を区別することが可能です。

```swift
import SomeModule

let myClass = MyModule.MyClass()
```


## 空白の使い方

領域の節約と不要な改行を避けるために、インデントにはタブではなく、2つの半角スペースを使用してください。そのため、Xcodeの環境設定で以下の画面の様に設定しておいてください。

![Xcode indent settings](screens/indentation.png)

メソッドや他の構文（`if`/`else`/`switch`/`while`など）の中括弧は、常にその元の文と同じ行で始めて、新規の行で終了してください。
**ヒント：**対象のコードを選択して（あるいは⌘A で全選択をして）から Control-I （またはメニューの Editor\Structure\Re-Indent）を実行すると、コードを整形することができます。Xcodeのテンプレートコードの中には4カラムのタブがハードコードされているものもありますが、こうすることで簡単にそれを修正することができます。 

**好ましい例：**
```swift
if user.isHappy {
  //何かをする
} else {
  //別な何かをする
}
```

**好ましくない例：**
```swift
if user.isHappy
{
    //何かをする
}
else {
    //別な何かをする
}
```

見た目の構造をわかりやくするために、メソッドとメソッドの間には1行だけ空白行を設けます。メソッド内の空白行は、処理のまとまりを分けるために使いますが、１つのメソッドの中が多くの部分に分かれている場合は、大抵、いくつかのメソッドに分割したほうが良いことを示しています。


## コメント

必要に応じて、**なぜ**	その部分のコードがその処理をしているのかをコメントしてください。コメントは常にコードの修正に合わせて修正するか、場合によっては削除してください。

本来、ソースコード本体をできるだけ分かりやすく書くべきで、コードの中にブロックコメントの形で何かを書くことは避けてください。*但しこれはリファレンス生成用のコメントには当てはまりません。*


## クラスと構造体

### どちらを使うべきか

構造体は、[値を表すもの](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_144)です。構造体は特定の個体の認識が必要のないものに使用してください。 [a, b, c] の要素からなる一つの配列は、別な、やはり [a, b, c] の要素からなる配列と等価で、交換しても全く差し支えありません。2つの配列の表している物が全く同じなので、１つ目の配列を使おうが、2つ目の配列を使おうが、全く問題ないのです。これが配列が構造体に属する理由です。

クラスは、[個体を参照するもの](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_145)です。クラスは特定の個体の認識が可能なもの、あるいは固有のライフサイクルを有するものに対して使用してください。2つの「人間」のオブジェクトは2つの異なるものを表していますので「人間」にはクラスを使います。二人の名前も誕生日も同じだったとしてもそれで二人が同じ「人間」にはなりません。しかし、1950年3月3日という日付は、別な1950年3月3日の日付オブジェクトと同じものですので、人間の「誕生日」は構造体になるでしょう。個々の日付オブジェクト自体には固有の識別は必要ありません。

なかには、本来は構造体として扱うべきものが、`AnyObject`を継承するために、あるいは歴史的な理由でクラスとして実装されているものもあります（`NSDate`、`NSSet`など）が、できるだけ上記のガイドラインに従ってください。

### 定義例

以下は、正しいスタイルで書かれたクラス定義の例です。

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

上の例では、以下のようなスタイルのガイドラインも示しています。

 + プロパティ、変数、定数、引数の宣言などの際に、型を指定する場合にはコロンの後に半角スペースを１つ挿入します。（コロンの前には挿入しません）例：`x: Int`、`Circle: Shape`
 + 同じ目的や意味合いの複数の変数を使用する場合には、1行にまとめて宣言します。
 + get節やset節の宣言、プロパティオブザーバもインデントします。
 + `internal`などのデフォルトで設定される修飾子をわざわざ付けないようにします。同様にメソッドをオーバーライドする場合のアクセス修飾子も付けないようにします。

### selfの使用

簡潔さを優先するために、selfは使用しないでください。Swiftの場合、プロパティへのアクセスやメソッドの呼び出しにselfを付ける必要はありません。

イニシャライザで引数名とプロパティ名を区別する必要がある場合、及びクロージャの中でプロパティを参照する場合（コンパイラに要求されます）には`self`を使います。 

```swift
class BoardLocation {
  let row: Int, column: Int

  init(row: Int,column: Int) {
    self.row = row
    self.column = column
    
    let closure = {
      println(self.row)
    }
  }
}
```

### プロトコルへの適合

クラスにプロトコルへの適合を追加する場合には、そのプロトコルのメソッド定義専用のクラス拡張を追加します。そうすることでそのプロトコルに関するメソッドが一箇所にまとまりますし、クラスにプロトコルへの適合とそのメソッドを追加する際の指針が単純になります。

また、ソースの閲覧性を良くするために、`// MARK: -`形式のコメントを忘れずに付けてください。

**好ましい例：**
```swift
class MyViewcontroller: UIViewController {
  //クラス固有の内容
}

// MARK: - UITableViewDataSource
extension MyViewcontroller: UITableViewDataSource {
  //テーブルビューのデータソースのメソッド
}

// MARK: - UIScrollViewDelegate
extension MyViewcontroller: UIScrollViewDelegate {
  //スクロールビューのデリゲートのメソッド
}
```

**好ましくない例：**
```swift
class MyViewcontroller: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
  //全てのメソッド
}
```

### 計算型プロパティ


簡潔さのために、計算型プロパティが読み取り専用であればget節は省略します。get節はset節が存在する場合にのみ必要になります。

**好ましい例：**
```swift
var diameter: Double {
  return radius * 2
}
```

**好ましくない例：**
```swift
var diameter: Double {
  get {
    return radius * 2
  }
}
```


## 関数の宣言

関数の宣言が短い場合は、開き中括弧も含めて1行に書くようにしてください。

```swift
func reticulateSplines(spline: [Double]) -> Bool {
  //関数本体
}
```

関数の宣言が長い場合には、適切な箇所で改行して、改行後の部分は通常より１つ余分にインデントしてください。

```swift
func reticulateSplines(spline: [Double], adjustmentFactor: Double,
    translateConstant: Int, comment: String) -> Bool {
  //関数本体
}
```


## クロージャの書き方

クロージャ式の引数が1つだけで引数リストの最後の引数である場合にのみ、接尾クロージャ形式の書き方を使用してください。クロージャの引数にはわかりやすい名称を付けてください。

**好ましい例：**
```swift
UIView.animateWithDuration(1.0) {
  self.myView.alpha = 0
}

UIView.animateWithDuration(1.0,
  animations: {
    self.myView.alpha = 0
  },
  completion: { finished in
    self.myView.removeFromSuperview()
  }
)
```

**好ましくない例：**
```swift
UIView.animateWithDuration(1.0, animations: {
  self.myView.alpha = 0
})

UIView.animateWithDuration(1.0,
  animations: {
    self.myView.alpha = 0
  }) { f in
    self.myView.removeFromSuperview()
}
```

1行形式のクロージャで処理内容が明確な場合には、return文を省略します。

```swift
attendeeList.sort { a, b in
  a > b
}
```


## 型

可能な場合には常にSwiftのネイティブな型を使用してください。SwiftではObjective-Cへのブリッジを提供しているので、必要であればObjective-Cのメソッドをすべて使用することができます。 

**好ましい例：**
```swift
let width = 120.0                                    //Double
let widthString = (width as NSNumber).stringValue    //String
```

**好ましくない例：**
```swift
let width: NSNumber = 120.0                                 //NSNumber
let widthString: NSString = width.stringValue               //NSString
```

Sprite Kit を使用するコードの中では、何度も型変換しないで済むことでコードが簡潔になるのであれば`CGFloat`を使用してください。

### 定数

定数には`let`キーワードを、変数には`var`キーワードを指定して宣言します。変数の値が変化しない場合には常に`var`ではなく`let`を使ってください。

**ヒント：** １つの簡単な方法は、全て`let`を使って宣言しコンパイラがエラーを出した場合だけそれを`var`に変更にするというものです。

### オプショナル型

nilになる可能性がある場合には、変数や関数の戻り値の型には`?`を付けてオプショナル型として宣言してください。

`!`を付けて宣言する暗黙的開示オプショナル型は、後から、実際に使用される前に、初期化されることがわかっているインスタンス変数（`viewDidLoad`の中で設定されるサブビューなど）の場合にだけ使用します。

オプショナル型の保持する値にアクセスする場合に、その値に1度しかアクセスしない場合や多数のオプショナル変数が連なっている場合には、オプショナル・チェインを使用してください。

```swift
self.textContainer?.textLabel?.setNeedsDisplay()
```

一度開示しておいて多くの処理で使用する方が便利な場合には、オプショナル束縛文を使用します。

```swift
if let textContainer = self.textContainer {
  //textContainerに対していろいろな処理を行う
}
```

オプショナル変数やオプショナル型のプロパティは、その型宣言でオプショナルであることは明示されているので、`optionalString`や`maybeView`のような名前を付けないようにします。 

オプショナル束縛文では、それが適切な場合にはオリジナルの名称を再利用しましょう。`unwrappedView`や`actualLabel`などの名前は使用しないようにします。

**好ましい例：**
```swift
var subview: UIView?
var volume: Double?

//後で...
if let subview = subview, volume = volume {
  //開示されたsubviewとvolumeを使って何かする
}
```

**好ましくない例：**
```swift
var optionalSubview: UIView?
var volume: Double?

if let unwrappedSubview = optionalSubview {
  if let realVolume = volume {
    // unwrappedSubviewとrealVolumeを使って何かする
  }
}
```

### 構造体の初期化

昔からあるCGGeometryの構築関数ではなく、Swiftの構造体のイニシャライザを使用するようにします。

**好ましい例：**
```swift
let bounds = CGRect(x: 40, y: 20, width: 120, height: 80)
let centerPoint = CGPoint(x: 96, y: 42)
```

**好ましくない例：**
```swift
let bounds = CGRectMake(40, 20, 120, 80)
let centerPoint = CGPointMake(96, 42)
```

グローバルな定数である`CGRectInfinite`や`CGRectNull`などではなく、構造体レベルの定数である`CGRect.infiniteRect`や`CGRect.nullRect`などを使用するようにしましょう。既存の変数に対しては、より短い`.zeroRect`も使用可能です。

### 型推定

コードをコンパクトにするためにも、CGFloat`や`Int16`のようにデフォルト以外の特殊な型が必要な場合を除き、定数や変数の型はコンパイラに推定させましょう。

**好ましい例：**
```swift
let message = "Click the button"
let currentBounds = computeViewBounds()
var names = [String]()
let maximumWidth: CGFloat = 106.5
```

**好ましくない例：**
```swift
let message: String = "Click the button"
let currentBounds: CGRect = computeViewBounds()
var names: [String] = []
```

**注意：**このガイドラインに準ずると、わかりやすい名前を付けることがより重要になってきます。

### シンタックス・シュガー

もともとの完全なジェネリクス形式の型ではなく、シンタックスシュガーの方を使用しましょう。

**好ましい例：**
```swift
var deviceModels: [String]
var employees: [Int: String]
var faxNumber: Int?
```

**好ましくない例：**
```swift
var deviceModels: Array<String>
var employees: Dictionary<Int, String>
var faxNumber: Optional<Int>
```


## 制御文

`for`ループには、`for-condition-increment`形式ではなく、`for-in`形式を使いましょう。

**好ましい例：**
```swift
for _ in 0..<3 {
  println("Hello three times")
}

for (index, person) in enumerate(attendeeList) {
  println("\(person) is at position #\(index)")
}
```

**好ましくない例：**
```swift
for var i = 0; i < 3; i++ {
  println("Hello three times")
}

for var i = 0; i < attendeeList.count; i++ {
  let person = attendeeList[i]
  println("\(person) is at position #\(i)")
}
```


## セミコロンの使用

Swiftでは文末のセミコロンは必要ありません。1行に複数の文を書きたい場合にのみ必要になりますが、その様な書き方はやめましょう。

この規則の唯一の例外は、`for-conditional-increment`構文で、この場合にはセミコロンが必要になります。しかし、可能な場合には代わりに`for-in`構文を使用すべきです。

**好ましい例：**
```swift
let swift = "not a scripting language"
```

**好ましくない例：**
```swift
let swift = "not a scripting language";
```

**注意：**JavaScriptの場合には、セミコロンを省略することは[一般的に危険](http://stackoverflow.com/questions/444080/do-you-recommend-using-semicolons-after-every-statement-in-javascript)であると考えられていますが、Swiftの場合には全くそんなことはありません。


## 言語

AppleのAPIに合わせて、米国英語のスペルを使用しましょう。

**好ましい例：**
```swift
let color = "red"
```

**好ましくない例：**
```swift
let colour = "red"
```


## 顔文字

顔文字は、raywenderlich.comのサイトの非常に大事な特質です！コードに関する満足感やワクワク感を正しい笑顔マークで表すのは非常に重要です。閉じ大括弧`]`が使われているのは、ASCII文字の中で最大の笑顔を表しているからです。閉じ小括弧`)`では、気乗り薄な笑顔になってしまって、望ましくありません。

**好ましい例：**
```
:]
```

**好ましくない例：**
```
:)
``` 


## クレジット

このスタイルガイドは、raywenderlich.comのチームの中でも最もスタイリッシュなメンバーたちによる協力の賜物です。

* [Jawwad Ahmad](https://github.com/jawwad)
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

[Nicholas Waynik](https://github.com/ndubbs) と [Objective-C Style Guide](https://github.com/raywenderlich/objective-c-style-guide) チームにも感謝!

AppleのSwiftに関するドキュメントも大いに参考にさせてもらいました。

* [The Swift Programming Language](https://developer.apple.com/library/prerelease/ios/documentation/swift/conceptual/swift_programming_language/index.html)
* [Using Swift with Cocoa and Objective-C](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/BuildingCocoaApps/index.html)
* [Swift Standard Library Reference](https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/SwiftStandardLibraryReference/index.html)
