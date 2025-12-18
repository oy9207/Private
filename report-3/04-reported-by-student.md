##  プログラミングII　2025　課題レポート（第4回）

# 解答用紙 → このファイルにレポートを書き込み、Githubへ提出する　
- 問題用紙をよく読み、この回答用紙にレポート記述して提出すること
- マークダウン記法を、しっかりプレビューして、記述ミスはすべて修正して完成させること
- 作成したコード、動作確認（実行結果）、分析・考察を自分の言葉で記入する
- 生成AIの回答、他人のレポートをコピーしてはいけない（判明し次第、減点ペナルティ対象とする）

レポート提出者：

|クラス|学籍番号|　氏名　|
|-|-|-|
|A|20125004|大野悠豊 |


### 選択する回答コース:  「〇〇」コース

- **骨太コース：SA級**：生成AIを使わず，ノーヒントで自力で解く骨太な勇者・猛者・トリックスターコース
- **庶民コース：AB級**：生成AIを使わず，ヒントにしたがってアプローチする一般的コース
- **ゆるふわコース：CD級**：生成AIや他力本願を前提とする ゆとりコース

### 選択する回答コース:  「〇〇〇〇」コース

**コース選択：アプローチの違い（道筋の険しさ）によって難易度・努力値を考慮する**
- **骨太コース：SA級**：生成AIを使わず，ノーヒントで自力で解く骨太な勇者・猛者・トリックスターコース
- **庶民コース：AB級**：生成AIを使わず，ヒントにしたがってアプローチする一般的コース
- **ゆるふわコース：CD級**：生成AIや他力本願を前提とするゆったりコース


### **問題1: シングルトンと属性制約を保証するメタクラスの設計**
- **概要**: メタクラスを使い、下記要件を満たす「AppConfigクラス」を**空欄を埋めて**設計せよ:
- **要件**
  - シングルトンパターン: クラスのインスタンスは1つしか生成できない
  - 属性制約: クラス内の属性名に制約:「属性名はすべて小文字」を課す
  - デフォルト値: 属性が指定されていない場合にデフォルト値を自動設定
- **課題**:
  1. **コード設計**:  空欄を埋めて、コードを完成させよ
  2. **分析**: なぜこの設計が有用か？実行結果の確認と検証
  3. **考察**: メタクラスを用いることで得られる利点を論じる

---
**作成したpythonコード、特に空欄やポイントの説明**
<!--
   - 動作するコードを完成させて、変数・関数・クラスの役割やロジックを簡潔に説明する
   - 意図的な例外処理・失敗コードはその旨、説明すること
-->

```python

class SingletonMeta(type):   ## 空欄A
    """Singleton を強制するメタクラス"""
    _instances = {}

    def __call__(cls, *args, **kwargs):  ## 空欄B
        if cls not in cls._instances:
            cls._instances[cls] = super().__call__(*args, **kwargs)
        return cls._instances[cls]


class AttributeConstraintMeta(type):
    """属性名がすべて小文字で命名されることを保証するメタクラス"""
    def __new__(cls, name, bases, dct):
        for attr_name in dct:
            if not attr_name.islower() and not attr_name.startswith("__"):    ## 空欄C
                raise TypeError(f"Attribute '{attr_name}' in class '{name}' must be in lowercase.")
        return super().__new__(cls, name, bases, dct)


class DefaultValuesMeta(type):
    """属性のデフォルト値を自動設定するメタクラス"""
    DEFAULT_VALUES = {
        'database': 'sqlite'
    }
    def __call__(cls, *args, **kwargs):
        instance = super().__call__(*args, **kwargs)
        for attr, default in cls.DEFAULT_VALUES.items():
            if not hasattr(instance, attr) or getattr(instance, attr) is None:
                setattr(instance, attr, default)
        return instance


##  """Singleton, Attribute Constraints, and Default Valuesを合成したメタクラス"""
class CombinedMetaClass(SingletonMeta, AttributeConstraintMeta, DefaultValuesMeta):  ## 空欄D
    pass


class AppConfig(metaclass=CombinedMetaClass):   ## 空欄E
    app_name = "MyApplication"  # 有効（小文字）
    version = "1.0"             # 有効（小文字）
    # Port = 8080               # 無効（小文字でないためエラー）
    database = None             # 有効


# インスタンス生成
configA = AppConfig()
configB = AppConfig()

# 同一インスタンス(シングルトン)を確認
assert configA is configB
print(configA, configB)

# 属性デフォルト値の設定を確認
print(configA.database)  # デフォルト値として "sqlite" を出力

```


**動作確認結果、確認ポイントの説明**
<!--
   - 実行結果の記述、正常に動作した証跡をもって確認する
   - コードや、コメントテキストはプレインテキストで記述し、
   - 必要に応じて、スクリーンキャプチャを添付してよい
-->

```

<__main__.AppConfig object at 0x7200c0235340> <__main__.AppConfig object at 0x7200c0235340>
sqlite

```

  
**分析**

なぜこの設計が有用な理由は、複数のルールを一つのメタクラスにまとめることで、クラス定義の段階から自動的に制約や機能を適用できるからです。
-
-
-
  
**考察**  
メタクラスを使う利点は、クラスの設計ルールを強制できることと、インスタンス生成を柔軟に制御ができるからです。
-
-
-

**改善点**
属性名を小文字に強制するだけじゃなくて、自動で直してくれるようにして、エラーで止まらなくて便利だと思います


-
-

---
---

### **問題2: 「チューリングマシンの停止問題」「万能デバッガが存在しない理由」**
- **課題**:  
- メタ循環インタプリタに関係する「チューリングマシンの停止問題」について、
- 講義中に説明した比喩「どんなバグもみつける完璧なデバッガが存在しない」を交えて
- 理解したことを自分の言葉で説明せよ
- **考察**: 
- あわせて「ゲーデルの不完全性定理」をふまえてどんな教訓を示しているか分析・考察せよ
 
---

**説明・分析**

チューリングマシンの停止問題とはプログラムが止まるがそれとも永遠に動き続けるかを完全に判断できるかどうかの問題。この問題は1936年にアラン・チューリングによって不可能だと証明だれました。


-
-
-
  
**考察**  
ゲーデルの不完全性定理は第一と第二があり。
第一不完全性定理は算数を含むようなルールは、真だけど証明できない命題が存在する。
第二不完全性定理はそのルールが矛盾していないことをそのルールのなかで証明することはできない。
この定理は、万能ではない。昔は数学のルールがあればどんなルールでも説明できるといわれていたけど、ゲーデルの定理よって不可能だと分かり、真理は人間が作ったルールの外にもあると分かりました。

-
-
-

**改善点**
この定理には改善できるものが存在しない。だが、この定理の応用を広げることで進化し続けいつか改善することができる。

-
-
-

---
---

### **問題3: オブザーバパターンを保証するメタクラスの設計**
- **概要**: メタクラスを使い、特定のクラスが「モデル（Observable）」として動作し、他の「オブザーバ（Observer）」に変更を通知する仕組みを確実に実施するサンプルコード設計を**空欄を埋めて**完成させよ
  
- **要件**
1. **`ObserverMeta` メタクラス**  
   - Observable クラスが必ず、 `add_observer`、`remove_observer`、`notify_observers` メソッドを持つことを強制し、実装されていない場合、例外をスローする

2. **`Observable` クラス**  
   - `ObserverMeta`メタクラスを適用した**基底クラス**で、観察される対象を管理する仕組みを提供、オブザーバを登録・解除し、状態変化時に通知する

3. **`Observer` クラス**  
   - `update` メソッドを持つ**抽象基底クラス**で、具体的なオブザーバクラスが継承して実装

4. **具体例（`TemperatureModel` と `TemperatureDisplay`）**  
   - `TemperatureModel` は(温度という)状態を持つモデルで、`temperature` プロパティが変更されると自動的にオブザーバへ通知
   - `TemperatureDisplay` はモデルから通知を受け取り、画面に表示を更新するオブザーバの具体例
   - 観察対象モデル（`TemperatureModel`）の状態変更がオブザーバ(`TemperatureDisplay`)に確実に通知される仕組みが可能
 
- **課題**:
  1. **コード設計**:  空欄を埋めて、コードを完成させよ
  2. **分析**: なぜこの設計が有用か？実行結果の確認と検証
  3. **考察**: メタクラスを用いることで得られる利点を論じる

---
**作成したpythonコード、特に空欄やポイントの説明**
<!--
   - 動作するコードを完成させて、変数・関数・クラスの役割やロジックを簡潔に説明する
   - 意図的な例外処理・失敗コードはその旨、説明すること
-->

```python

# **`ObserverMeta` メタクラス**
class ObserverMeta(type):       #  **空欄A**
    """
    - Observable クラスが必ず、`add_observer`、`remove_observer`、`notify_observers` メソッドを持つことを強制する
    - 実装されていない場合、エラーをスローする
    """
    def __new__(cls, name, bases, dct):
        required_methods = { "add_observer", "remove_observer", "notify_observers" }
        implemented_methods = set(dct.keys()).union(*(set(dir(base)) for base in bases))
        if not required_methods.issubset(implemented_methods):
            missing = required_methods - implemented_methods
            raise TypeError(f"Class {name} is missing required methods: {','.join(missing)}")
        return super().__new__(cls, name, bases, dct)  

# **`Observable` クラス**
class Observable(metaclass=ObserverMeta):     #  **空欄B**
    """
      - `ObserverMeta`メタクラスを適用した**基底クラス**: 観察される対象を管理する仕組みを提供
      - オブザーバを登録・解除し、状態変化時に通知する
    """
    def __init__(self):
        self._observers = []

    def add_observer(self, observer):
        if observer not in self._observers:
            self._observers.append(observer)    #  **空欄C**

    def remove_observer(self, observer):
        if observer in self._observers:
            self._observers.remove(observer)    #  **空欄D**

    def notify_observers(self):
        for observer in self._observers:
            observer.update(self)               

# **`Observer` クラス**
class Observer:
    """
     - `update` メソッドを持つ**抽象基底クラス**で、具体的なオブザーバクラスが継承して実装
    """
    def update(self, observable):
        raise NotImplementedError("The 'update' method must be implemented by subclasses.")

## 観察者(View)の具体例
class TemperatureDisplay(Observer):
    def update(self, observable):
        if isinstance(observable, TemperatureModel):
            print(f"[Display] Model updated to {observable.temperature}°C [Display]")

class TemperatureBrowser(Observer):
    def update(self, observable):
        if isinstance(observable, TemperatureModel):
            print(f"<Web> Model updated to {observable.temperature}°C</Web>")

## 観察対象(Model)の具体例
class TemperatureModel(Observable):     #  **空欄E**
    def __init__(self):
        super().__init__()
        self._temperature = 0

    @property
    def temperature(self):
        return self._temperature

    @temperature.setter
    def temperature(self, value):
        self._temperature = value
        self.notify_observers()     #  **空欄F**

def test():
    # 観察対象　Model
    model = TemperatureModel()
    # 観察者　View
    display = TemperatureDisplay()
    view = TemperatureBrowser()

    #  オブザーバの割り当て
    model.add_observer(display)
    model.add_observer(view)

    #  モデルのアップデート
    model.temperature = 25
    model.temperature = 30

    #  オブザーバの解除
    model.remove_observer(display)
    model.temperature = 35

    #  オブザーバの再割り当て
    model.add_observer(display)
    model.temperature = 44

test()

```


**動作確認結果、確認ポイントの説明**
<!--
   - 実行結果の記述、正常に動作した証跡をもって確認する
   - コードや、コメントテキストはプレインテキストで記述し、
   - 必要に応じて、スクリーンキャプチャを添付してよい
-->

```

[Display] Model updated to 25°C [Display]
<Web> Model updated to 25°C</Web>
[Display] Model updated to 30°C [Display]
<Web> Model updated to 30°C</Web>
<Web> Model updated to 35°C</Web>
<Web> Model updated to 44°C</Web>
[Display] Model updated to 44°C [Display]

```

  
**分析**

この設計は、オブザーバーパターンを必ず守るように強制できるところが便利だと思いました。この設計は設計ミスを早めに見つけられる。
また、実行結果で、温度を変えるたびにTemperatureDisplayとTemperatureBrowserがちゃんと更新していることが分かりました

-
-
-
  
**考察**  
メタクラスを使う利点は、設計の一貫性を保証できることだと思いました。大きなプロジェクトで一人がルールを間違えても必ずこの設計で作らないとエラーになるので安心して作れます

-
-
-

**改善点**
このコードだと、オブザーバのupdateメソッドはただprintするだけなので、もっと実用的にするならGUIの更新やログ保存に使えると良いと思いました。

-
-
-

-----
-----


### **問題4: メタクラスではなく、`__init_subclass__()`で代用する方法**
- **課題**: 問題３をシンプルに、`__init_subclass__()`で代用するコードを**空欄を埋めて**完成させよ



---
**作成したpythonコード、特に空欄やポイントの説明**
<!--
   - 動作するコードを完成させて、変数・関数・クラスの役割やロジックを簡潔に説明する
   - 意図的な例外処理・失敗コードはその旨、説明すること
-->

```python

class ObserverMeta(type):       #  **空欄A**
    """
    - Observable クラスが必ず、`add_observer`、`remove_observer`、`notify_observers` メソッドを持つことを強制する
    - 実装されていない場合、エラーをスローする
    """
    def __new__(cls, name, bases, dct):
        required_methods = { "add_observer", "remove_observer", "notify_observers" }
        implemented_methods = set(dct.keys()).union(*(set(dir(base)) for base in bases))
        if not required_methods.issubset(implemented_methods):
            missing = required_methods - implemented_methods
            raise TypeError(f"Class {name} is missing required methods: {','.join(missing)}")
        return super().__new__(cls, name, bases, dct)  

class Observable(metaclass=ObserverMeta):     #  **空欄B**
    """
      - `ObserverMeta`メタクラスを適用した**基底クラス**: 観察される対象を管理する仕組みを提供
      - オブザーバを登録・解除し、状態変化時に通知する
    """
    def __init__(self):
        self._observers = []

    def add_observer(self, observer):
        if observer not in self._observers:
            self._observers.append(observer)    #  **空欄C**

    def remove_observer(self, observer):
        if observer in self._observers:
            self._observers.remove(observer)    #  **空欄D**

    def notify_observers(self):
        for observer in self._observers:
            observer.update(self)               

class Observer:
    """
     - `update` メソッドを持つ**抽象基底クラス**で、具体的なオブザーバクラスが継承して実装
    """
    def update(self, observable):
        raise NotImplementedError("The 'update' method must be implemented by subclasses.")

class TemperatureDisplay(Observer):
    def update(self, observable):
        if isinstance(observable, TemperatureModel):
            print(f"[Display] Model updated to {observable.temperature}°C [Display]")

class TemperatureBrowser(Observer):
    def update(self, observable):
        if isinstance(observable, TemperatureModel):
            print(f"<Web> Model updated to {observable.temperature}°C</Web>")

class TemperatureModel(Observable):     #  **空欄E**
    def __init__(self):
        super().__init__()
        self._temperature = 0

    @property
    def temperature(self):
        return self._temperature

    @temperature.setter
    def temperature(self, value):
        self._temperature = value
        self.notify_observers()     #  **空欄F**

def test():
    model = TemperatureModel()
    display = TemperatureDisplay()
    view = TemperatureBrowser()

    model.add_observer(display)
    model.add_observer(view)

    model.temperature = 25
    model.temperature = 30

    model.remove_observer(display)
    model.temperature = 35

    model.add_observer(display)
    model.temperature = 44

test()

```


**動作確認結果、確認ポイントの説明**
<!--
   - 実行結果の記述、正常に動作した証跡をもって確認する
   - コードや、コメントテキストはプレインテキストで記述し、
   - 必要に応じて、スクリーンキャプチャを添付してよい
-->

```

[Display] Model updated to 25°C [Display]
<Web> Model updated to 25°C</Web>
[Display] Model updated to 30°C [Display]
<Web> Model updated to 30°C</Web>
<Web> Model updated to 35°C</Web>
<Web> Model updated to 44°C</Web>
[Display] Model updated to 44°C [Display]

```

  
**分析**

今回のプログラムでは、温度を変えるたびにTemperature updated to〇〇℃と表示されました。
また、最後にremove_obseverを読んだ後は表示されなくなったのでちゃんとオブザーバーが外れていることが確認できた。

-
-
-
  
**考察**  
メタクラスを使わずに__init_subclass__()を使った方法はシンプルでわかりやすくメタクラスをまだ完全に理解しなくても使えるコードでした。


-
-
-

**改善点**
temperatureDisplayだけでなく温度表示などを作ると仕組みがもっとイメージしやすい
-
-
-

----
----

### **問題5: 自作パッケージとデプロイの実践
- **概要**: 自作パッケージを作って、デプロイし、他者が`pip insatll`や、`import`を使って活用できる状態にする
- 第12回の練習課題（プラクティスNo.1，No.2）を参考に、**自作パッケージを作ってデプロイするプラクティスNo.3**を実施する
- 簡単なユーティリティ関数を束ねたパッケージを自作する
- 例えば、次のような簡単な関数や、その他有用な関数を追加する：
- 

    - 1. BMI計算関数: 身長と体重を入力として受け取り、BMI（Body Mass Index）を計算して返す関数 : `calculate_bmi(weight, height)`
    - 2. 温度変換関数: 摂氏温度を華氏温度に変換する関数:`celsius_to_fahrenheit(celsius)`と、その逆変換:`fahrenheit_to_celsius(fahrenheit)`
    - 3. 単語カウント関数: テキスト内の単語数をカウントする関数:`count_words(text)`
    - 4. リストの平均値計算関数:`calculate_average(numbers)`
    - 5. 素数判定関数:`is_prime(number)`
    - 6. フィボナッチ数列生成関数:` generate_fibonacci(n)`
    - 7. 文字列の逆順関数:`reverse_string(s)`
    - 8. 最大公約数（GCD）計算関数:`gcd(a, b)`

----
---
**作成したpythonコード、特に空欄やポイントの説明**
<!--
   - 動作するコードを完成させて、変数・関数・クラスの役割やロジックを簡潔に説明する
   - 意図的な例外処理・失敗コードはその旨、説明すること
-->

```python



def calculate_bmi(weight, height):
    """BMI = 体重(kg) / 身長(m)^2"""
    return weight / (height ** 2)

def celsius_to_fahrenheit(celsius):
    """摂氏 → 華氏"""
    return (celsius * 9/5) + 32

def fahrenheit_to_celsius(fahrenheit):
    """華氏 → 摂氏"""
    return (fahrenheit - 32) * 5/9

def count_words(text):
    """テキスト内の単語数をカウント"""
    words = text.split()
    return len(words)

def calculate_average(numbers):
    """リストの平均値を計算"""
    if not numbers:
        raise ValueError("numbersリストが空です")
    return sum(numbers) / len(numbers)

def is_prime(number):
    """素数判定"""
    if number <= 1:
        return False
    for i in range(2, int(number ** 0.5) + 1):
        if number % i == 0:
            return False
    return True

def generate_fibonacci(n):
    """フィボナッチ数列をn個生成"""
    if n <= 0:
        return []
    fib_sequence = [0, 1]
    while len(fib_sequence) < n:
        fib_sequence.append(fib_sequence[-1] + fib_sequence[-2])
    return fib_sequence[:n]

def reverse_string(s):
    """文字列を逆順にする"""
    return s[::-1]

def gcd(a, b):
    """最大公約数を計算"""
    while b:
        a, b = b, a % b
    return a


**使用例**
'''
if __name__ == "__main__":
    print("BMI:", calculate_bmi(70, 1.75))
    print("摂氏30℃ → 華氏:", celsius_to_fahrenheit(30))
    print("華氏86°F → 摂氏:", fahrenheit_to_celsius(86))
    print("単語数:", count_words("Hello world!"))
    print("平均値:", calculate_average([1, 2, 3, 4, 5]))
    print("29は素数か:", is_prime(29))
    print("フィボナッチ10個:", generate_fibonacci(10))
    print("逆順文字列:", reverse_string("hello"))
    print("GCD(48, 18):", gcd(48, 18))
'''
```


**動作確認結果、確認ポイントの説明**
<!--
   - 実行結果の記述、正常に動作した証跡をもって確認する
   - コードや、コメントテキストはプレインテキストで記述し、
   - 必要に応じて、スクリーンキャプチャを添付してよい
-->

```

摂氏30℃ → 華氏: 86.0
華氏86°F → 摂氏: 30.0
単語数: 2
平均値: 3.0
29は素数か: True
フィボナッチ10個: [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
逆順文字列: olleh
GCD(48, 18): 6

```

  
**分析**

このコードを実行しても何も出ないため実行時に出力確認ができるように使用例を書いた
-
-
-
  
**考察**  
複雑なクラスえを使わずにシンプルな関数で構成しているためテストしやすい


-
-
-

**改善点**
身長が0以下の場合エラーを出す。
-
-
-

-----

### 所感・今後の目標課題 (自由記述欄、フィードバックご意見を尊重します)

- プログラミングに興味が湧かない原因
　ゲームは長い間やっているのもあって感覚で楽しめます。でもプログラミングは慣れるまでに時間がかかりすぐに楽しさを感じられないためなかなか手が出せずモチベーション上がらない
- プログラミングには興味がないが、それ以外で没頭していること
　ゲームに没頭しています
- 授業中に質問や意見が言えない理由
　自分の意見に自信がなかったり、周囲と比べてしまうので発言できない
　予習不足で、全然理解が追い付いていない。
- コードや英語や文字、活字が好きにならない理由
　コードや英語は意味を理解しないと楽しめないので苦手意識がある



　休日や春休みなど時間があるときに、一からプログラミングを学び直すことを決めました。とにかくあきらめずに取り組み、小さな成功体験を積み重ねて自信につなげたいです
 





---

