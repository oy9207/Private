##  プログラミングII　2025　課題レポート（第3回）

# 解答用紙 → このファイルにレポートを書き込み、Githubへ提出する　
- 問題用紙をよく読み、この回答用紙にレポート記述して提出すること
- マークダウン記法を、しっかりプレビューして、記述ミスはすべて修正して完成させること
- 作成したコード、動作確認（実行結果）、分析・考察を自分の言葉で記入する
- 生成AIの回答、他人のレポートをコピーしてはいけない（判明し次第、減点ペナルティ対象とする）

レポート提出者：

|クラス|学籍番号|　氏名　|
|-|-|-|
|A|20125004|大野悠豊|

# 解答編

- それぞれの問題をよく読み、以下の項目に自分の言葉でしっかり記述する
- マークダウン記法を、しっかりプレビューして、記述ミスはすべて修正して完成させる

### 選択する回答コース:  「〇〇」コース

- **骨太コース：SA級**：生成AIを使わず，ノーヒントで自力で解く骨太な勇者・猛者・トリックスターコース
- **庶民コース：AB級**：生成AIを使わず，ヒントにしたがってアプローチする一般的コース
- **ゆるふわコース：CD級**：生成AIや他力本願を前提とする ゆとりコース



### **問題1: ジェネレータを使ったコルーチンの設計と実装**
- **概要**: Pythonジェネレータを使って生産者-消費者問題を解決するコードを設計せよ
- **課題内容**:
  1. 要件：生産者コルーチンがデータを生成し、消費者コルーチンがデータを消費するコード
  2. `send`と`receive = yield`を用いた協調的並列処理の実装
  3. **分析**: なぜこの設計が必要か？実行結果の確認と検証
  4. **考察**: コルーチンを用いることで得られる効率性や設計上の利点を論じる
  5. **応用（チャレンジ）** 　ジェネレータに対する`StopIteration`例外を使うことで、生産者が生産を終了した後、消費者も消費を停止するコードに進化せよ

---
**作成したpythonコード、特に空欄やポイントの説明**
<!--
   - 動作するコードを完成させて、変数・関数・クラスの役割やロジックを簡潔に説明する
   - 意図的な例外処理・失敗コードはその旨、説明すること
-->

```python

import time
import random


def consumer():
    print("<消費者>: 準備OK...")
    try:
        while True:
            item = yield   
            print(f"<消費者>: {item} 受領… 消費中………")
            duration = random.uniform(0.5, 1.5)
            time.sleep(duration)
            print(f"<消費者>: 所要時間{duration:.2f}秒で {item} 消費完了")
    except StopIteration:
        print("<消費者>: 消費完了")


def producer(consumer_coroutine):
    print("[生産者]: 生産準備OK...")
    next(consumer_coroutine)  
    for i in range(3):  
        duration = random.uniform(0.5, 1.5)
        time.sleep(duration)
        item = f"アイテム {i}"
        print(f"\n---[生産者]: 所要時間{duration:.2f}秒で {item} を生産")
        consumer_coroutine.send(item)  
    consumer_coroutine.close()  
    print("\n---[生産者]: 生産終了")

if __name__ == "__main__":
    c = consumer()
    producer(c)

```


**動作確認結果、確認ポイントの説明**
<!--
   - 実行結果の記述、正常に動作した証跡をもって確認する
   - コードや、コメントテキストはプレインテキストで記述し、
   - 必要に応じて、スクリーンキャプチャを添付してよい
-->

```

[生産者]: 生産準備OK...
<消費者>: 準備OK...

---[生産者]: 所要時間1.39秒で アイテム 0 を生産
<消費者>: アイテム 0 受領… 消費中………
<消費者>: 所要時間1.19秒で アイテム 0 消費完了

---[生産者]: 所要時間1.27秒で アイテム 1 を生産
<消費者>: アイテム 1 受領… 消費中………
<消費者>: 所要時間1.49秒で アイテム 1 消費完了

---[生産者]: 所要時間1.01秒で アイテム 2 を生産
<消費者>: アイテム 2 受領… 消費中………
<消費者>: 所要時間0.55秒で アイテム 2 消費完了

---[生産者]: 生産終了

```

  
**分析**

yieldとsend()を使うことで、関数同士が交互に動いているように見えます。普通に書くと生産してから全部まとめて消費するになりがちですが、この方法だと一つずつ渡して処理できるので、流れが分かりやすいと思いました。

-
-
-
  
**考察**  
コルーチンを使うと、同時に動いているような感じを簡単に作れるのが便利だと思いました。
-
-
-

**改善点**
今回のコードはアイテムを3つだけなので「生産者が何個作るかを外から指定できるようにする」とか、「消費者が処理した結果を返すようにする」とかを追加する。

-
-
-

---
---

### **問題2: 基本的なオブジェクト指向の例題**
- **概要**: 図形クラスとその派生クラスを設計し、オブジェクト指向の原則の理解度を確認する
- **課題内容**:
  1. 抽象基底クラス`Shape`を定義し、以下のクラスを継承して設計実装せよ
     - 四角形クラス`Rectangle`
     - 三角形クラス`Triangle`
     - 楕円クラス`Ellipse`
     - 五角形クラス`Pentagon`
  2. 各クラスに`rotate`メソッドを実装し、「指定角度（degree）だけ時計回りに回転させる」機能を追加しなさい（シミュレーションでよい、厳密な行列・ベクトルを用いた幾何学的回転の実装は不要）
  3. ポリモーフィズムの概念を用いた操作を記述せよ
  4. **考察**: 継承、抽象基底クラスによるインターフェースの強制、ポリモーフィズムのメリットと、設計上の効果を分析しなさい



---
**作成したpythonコード、特に空欄やポイントの説明**
<!--
   - 動作するコードを完成させて、変数・関数・クラスの役割やロジックを簡潔に説明する
   - 意図的な例外処理・失敗コードはその旨、説明すること
-->

```python

from abc import ABC, abstractmethod


class Shape(ABC):
    @abstractmethod
    def rotate(self, degree):
        pass


class Rectangle(Shape):
    def rotate(self, degree):
        print(f"Rectangleを{degree}度回転しました。")


class Triangle(Shape):
    def rotate(self, degree):
        print(f"Triangleを{degree}度回転しました。")


class Ellipse(Shape):
    def rotate(self, degree):
        print(f"Ellipseを{degree}度回転しました。")


class Pentagon(Shape):
    def rotate(self, degree):
        print(f"Pentagonを{degree}度回転しました。")


shapes = [Rectangle(), Triangle(), Ellipse(), Pentagon()]

for s in shapes:
    s.rotate(90)   

```


**動作確認結果、確認ポイントの説明**
<!--
   - 実行結果の記述、正常に動作した証跡をもって確認する
   - コードや、コメントテキストはプレインテキストで記述し、
   - 必要に応じて、スクリーンキャプチャを添付してよい
-->

```

Rectangleを90度回転しました。
Triangleを90度回転しました。
Ellipseを90度回転しました。
Pentagonを90度回転しました。

```

  
**分析**

共通の枠組みを作れるので、図形ごとに同じインターフェースを持たせられる。

-
-
-
  
**考察**  
抽象クラスShapeを作ることで、すべての図形に回転できるという共通の約束を持たせられました。継承を使うとコードの重複が減り、ポリモーフィズムによって同じ操作を統一的に扱えるのが便利だと思いました。


-
-
-

**改善点**
今回は回転しましたと表示するだけなので、実際の座標変換や描画まではできていないが将来的には座標やベクトルを使った本格的な回転処理を追加してみたい

-
-
-

---

### **問題3: **コマンド(Command)** デザインパターンの実装**
- **概要**: テキストエディタに対する編集操作と`Undo`/`Redo`機能をコマンドデザインパターンで実装せよ
- **課題内容**:
  1. 基本的なコマンドクラス`Command`を設計
  2. 具体的なコマンドとして、`InsertText`、`DeleteText`を実装しなさい（シミュレーションでよい）
  3. `Undo`/`Redo`機能を有効にする履歴管理を追加しなさい
  4. **考察**: コマンドパターンがもたらす柔軟性と拡張性について分析

---
**作成したpythonコード、特に空欄やポイントの説明**
<!--
   - 動作するコードを完成させて、変数・関数・クラスの役割やロジックを簡潔に説明する
   - 意図的な例外処理・失敗コードはその旨、説明すること
-->

```python

class Command:
    def execute(self):
        pass
    def undo(self):
        pass

class TextEditor:
    def __init__(self):
        self.text = ""


class InsertText(Command):
    def __init__(self, editor, pos, content):
        self.editor = editor
        self.pos = pos
        self.content = content

    def execute(self):
    
        self.editor.text = self.editor.text[:self.pos] + self.content + self.editor.text[self.pos:]

    def undo(self):
   
        self.editor.text = self.editor.text[:self.pos] + self.editor.text[self.pos+len(self.content):]


class DeleteText(Command):
    def __init__(self, editor, pos, length):
        self.editor = editor
        self.pos = pos
        self.length = length
        self.deleted = ""

    def execute(self):
        
        self.deleted = self.editor.text[self.pos:self.pos+self.length]
        self.editor.text = self.editor.text[:self.pos] + self.editor.text[self.pos+self.length:]

    def undo(self):
      
        self.editor.text = self.editor.text[:self.pos] + self.deleted + self.editor.text[self.pos:]


class CommandHistory:
    def __init__(self):
        self.undo_stack = []
        self.redo_stack = []

    def execute(self, cmd):
        cmd.execute()
        self.undo_stack.append(cmd)
        self.redo_stack.clear()

    def undo(self):
        if len(self.undo_stack) > 0:
            cmd = self.undo_stack.pop()
            cmd.undo()
            self.redo_stack.append(cmd)

    def redo(self):
        if len(self.redo_stack) > 0:
            cmd = self.redo_stack.pop()
            cmd.execute()
            self.undo_stack.append(cmd)


editor = TextEditor()
history = CommandHistory()

history.execute(InsertText(editor, 0, "Hello"))
history.execute(InsertText(editor, 5, " World"))
print(editor.text)  


history.undo()
print(editor.text) 


history.redo()
print(editor.text) 

```


**動作確認結果、確認ポイントの説明**
<!--
   - 実行結果の記述、正常に動作した証跡をもって確認する
   - コードや、コメントテキストはプレインテキストで記述し、
   - 必要に応じて、スクリーンキャプチャを添付してよい
-->

```

Hello World
Hello
Hello World

```

  
**分析**

今回のコードでは、テキストエディタに対する操作をコマンドとしてクラスに分けて実装しました。InsertTextとDeleteTextの2種類の操作を作り、それぞれexecute()とundo()を持たせることで、Undo/Redoができるようになっています。


-
-
-
  
**考察**  
コマンドパターンを使うと、操作を「オブジェクト」として扱えるので、UndoやRedoがすごくやりやすくなりました。普通に関数を呼ぶだけだと戻す処理を自分で考えないといけないけど、コマンドにundo()を持たせることで自然に履歴管理ができるのが便利だと思いました


-
-
-

**改善点**
削除するときに範囲外の位置を指定したらエラーが出て止まります。エラー処理をするために「try/except」でエラーを防ぐ工夫をした方が安心です。

-
-
-

-----
-----

### **問題4: オブザーバ(Observer) デザインパターンの実装**
- **概要**: Pythonのダックタイピングを活用して**オブザーバパターン**を実装し、MVCモデルについて論じよ
- **課題内容**:
  1. `Observer`と`Subject`を設計し、観察者-被観察者関係を構築
  2. サンプルアプリとして簡易的なMVCモデルを実装
  3. 各コンポーネントの役割を論述
  4. **考察**: ダックタイピングの利点と、オブザーバパターンによる柔軟なシステム設計についてパターンを使わない場合と比較し，効能・使い道，論じる

---
**作成したpythonコード、特に空欄やポイントの説明**
<!--
   - 動作するコードを完成させて、変数・関数・クラスの役割やロジックを簡潔に説明する
   - 意図的な例外処理・失敗コードはその旨、説明すること
-->

```python

class Subject:
    def __init__(self):
        self.observers = []

    def add_observer(self, obs):
        self.observers.append(obs)

    def notify(self):
        for obs in self.observers:
            obs.update(self)

class Model(Subject):
    def __init__(self):
        super().__init__()
        self.data = ""

    def set_data(self, value):
        self.data = value
        self.notify()

class View:
    def update(self, subject):
        print("View: データが更新されました ->", subject.data)

class Controller:
    def __init__(self, model):
        self.model = model

    def change_data(self, value):
        self.model.set_data(value)

model = Model()
view = View()
controller = Controller(model)

model.add_observer(view)

controller.change_data("Hello Observer")
controller.change_data("MVCモデルのテスト")

```


**動作確認結果、確認ポイントの説明**
<!--
   - 実行結果の記述、正常に動作した証跡をもって確認する
   - コードや、コメントテキストはプレインテキストで記述し、
   - 必要に応じて、スクリーンキャプチャを添付してよい
-->

```

View: データが更新されました -> Hello Observer
View: データが更新されました -> MVCモデルのテスト

```

  
**分析**

今回のコードでは、Subjectが通知する側、Observerが通知を受ける側としてつくりました。Pythonのダックタイピングを使ってupdateメソッドを持っていればObserverとみなすことで、インターフェースを定義しなくても動くようになっています。


-
-
-
  
**考察**  
Pythonのダックタイピングを使うと、Observerインターフェースをわざわざ定義しなくてもupdate メソッドを持っているなら Observerとして扱えるので、コードがシンプルになります。。

-
-
-

**改善点**
Observerがupdate()を持っていなかったらエラーになになるので、エラー処理を追加する

-
-
-

----
----

### **チャレンジ問題5: 非同期処理の実装と分析**　（骨太コース必須）
- **概要**: `async`と`await`を使用した非同期処理の設計と実装を通じて、非同期プログラミングを理解する
- **課題内容**:
  1. `async`と`await`を使用して複数の非同期タスクを並列実行するコード（例：Webスクレイピング、ファイル入出力）のサンプル例を実行し、動作原理を理解する
  2. 実行時間の比較実験を行い、同期処理との差を示す
  3. 非同期処理の利点、欠点、適用例を分析し、同期処理との違いを考察して，自分の言葉で説明せよ

----
---
**作成したpythonコード、特に空欄やポイントの説明**
<!--
   - 動作するコードを完成させて、変数・関数・クラスの役割やロジックを簡潔に説明する
   - 意図的な例外処理・失敗コードはその旨、説明すること
-->

```python

ここにコードを書く

```


**動作確認結果、確認ポイントの説明**
<!--
   - 実行結果の記述、正常に動作した証跡をもって確認する
   - コードや、コメントテキストはプレインテキストで記述し、
   - 必要に応じて、スクリーンキャプチャを添付してよい
-->

```

ここに実行結果を書く

```

  
**分析**

<!--
   - 出題者の意図を察知し、どんな概念・技法を理解し、修得する問題か、自分の言葉で簡潔にまとめる
-->

-
-
-
  
**考察**  
<!--
   - 問題解決の考え方・攻略法を自分の言葉で簡潔にまとめる
   - 用いた技法、例えば、関数プログラミング、プロトコル、オブジェクト指向等の技法の効果、活用場面について、応用シーンを考察する
-->

-
-
-

**改善点**
<!--
   - この課題を通して、まだ改善できると感じた点や他の解法のアイデアを自分の言葉で論じる
-->

-
-
-

-----

### 所感・今後の目標課題
 (自由記述欄、多様なご意見を尊重します)
 今回の課題を通して、オブザーバーパターンの仕組みを体験できました。実際にパターンを使ってみるとModelとViewが緩くつながることで拡張しやするることが分かりました。
今後はコマンドパターンやストラテジーパターンと組み合わせて複雑なアプリを作ってみたいです。


 

---

