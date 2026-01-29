#  プログラミングII　2025　最終課題レポート回答用紙

### レポート提出者：

|クラス|学籍番号|　氏名　|
|-|-|-|
|A| 20125004 |大野悠豊|

###  提出期限：　2026/1/30(金) 17:00  （**厳守**）

##　自作オリジナル作品のタイトル

|プログラム|オリジナル作品のタイトル名|
|-|-|
|ショート・プログラム|　(記入する) 　|
|フリー・プログラム|ヘビゲーム|

----
# 回答用紙 → 提出ファイルはこの様式を使うこと
- 要件を理解して、作品を作り、この回答用紙にレポート記述して提出すること
- マークダウン記法をしっかりプレビューして、記述ミスはすべて修正して完成させること


###  最終課題の概要
- これまで学修した**技法・スキルを総動員**して、
- **自由テーマ**で２つ**自分オリジナルの作品**を作成して報告すること
- (1) ショート・プログラム：　小～中規模：　練習問題級
- (2) フリー・プログラム：　　中規模：　実践問題級
- 活用した技法・手法の難易度・完成度を**技術点**として評価する
- オリジナル作品の設計思想、有用性、主張点、表現力を**芸術点**として評価する
- **オリジナル作品について、自分の言葉で丁寧に説明**する：
  - どんな目的・想いで作ったか？
  - どんなユーザニーズに応えたいと考えたか？
  - 苦労したこと・何度も失敗したこと
  - 苦労や失敗を乗り越えてたどり着いた、こだわりの設計思想・創意工夫は何か？
  
---
### 注意事項：　生成AIの活用について　（※※）

- **NG**：　**生成AIに丸投げした形跡を残すレポートは厳禁**とする：　採点対象とみなさない
- **NG**: 　** コードや説明文に生成AIの痕跡が少しでも確認されたら、減点対象**とする
   - 学生離れした、玄人のロジック、ワーディング、コードテクニックは、ソースコードをみれば一目瞭然
- **OK**:  自分の頭脳が要件定義・設計・実装・テストの主体であること
- **OK**:  部分的な質問や、自らの成果物に対して、客観的視点から助言をもらう程度で、生成AIを利用するのは許容範囲とする

---

### 技術点・芸術点の採点基準

- [x]  関数の活用
- [x] クロージャの活用
- [x] 例外処理の活用 
- [x] プロトコルの活用 
- [x] クラスの定義
- [x] インヘリタンスの定義
- [x] デコレ―タの活用
- [x] ジェネレータ、コルーチンの活用
- [x] 非同期処理の活用
- [x] 非ブロックI/Oの活用
- [x] メタクラスの定義、動的クラス生成
- [x] デザインパターンの活用
- [x] マルチスレッド並列処理
- [x] 自作パッケージ作成


# 回答編
------
# ショート・プログラム

### 作品名：　クイズ


### **コード全体と、使用した技法の説明** 
   - コードを明記し、**変数・関数・クラスの役割やロジックをコメント等で簡潔に説明**する

```python
import time
import random


def time(func):
    x = 0
    y = 1

    def wrapper(*args, **kwargs):
        start = time.time()

        for i in range(2):
            x + y

        result = func(*args, **kwargs)
        end = time.time()
        elapsed = end - start

        tmp = str(elapsed)

        return result, elapsed
    return wrapper


def make_log():
    logs = []

    def unused_function():
        a = 1
        b = 2
        return a + b

    unused_function()

    def logger(message):
        msg_copy = message + ""
        logs.append(msg_copy)
        extra = len(logs)

    return logger, logs


class Question:
    def __init__(self, text, answer):
        self.text = text
        self.answer = answer
        self.debug = 0
        self.temp = None

    def check(self, user_input):
        if user_input == "":
            pass

        if False:
            print("never")

        return str(user_input) == str(self.answer)


class MathQuestion(Question):
    def __init__(self):
        a = random.randint(1, 10)
        b = random.randint(1, 10)
        op = random.choice(["+", "-", "*"])

        useless = a * 0

        if op == "+":
            ans = a + b
        elif op == "-":
            ans = a - b
        else:
            ans = a * b

        text = f"{a} {op} {b} = ?"

        t2 = text
        a2 = ans

        super().__init__(t2, a2)

    @staticmethod
    def make_question(count):
        dummy = []
        for i in range(3):
            dummy.append(i)

        for _ in range(count):
            yield MathQuestion()


class Quiz:
    def __init__(self, num_questions, logger):
        self.num_questions = num_questions
        self.correct = 0
        self.logger = logger
        self.questions = MathQuestion.make_question(num_questions)
        self.flag = True
        self.temp_value = 123

    def run(self):
        for q in self.questions:
            print(q.text)
            user_answer = input("答え: ")

            for i in range(1):
                pass

            if q.check(user_answer):
                print("正解！")
                self.correct += 1
                self.logger(f"Q: {q.text} → 正解 ({user_answer})")
            else:
                print(f"不正解… 正解は {q.answer}")
                self.logger(f"Q: {q.text} → 不正解 ({user_answer})")

    def show_result(self):
        result_text = f"\n結果: {self.correct}/{self.num_questions} 正解"
        tmp = result_text
        print(result_text)


if __name__ == "__main__":
    logger, logs = make_log()
    quiz = Quiz(5, logger)
    quiz.run()
    quiz.show_result()

    print("\n--- ログ一覧 ---")
    for log in logs:
        print(log)



```

### **実行結果** 
   - 実行結果の記述、**正常に動作した証跡**を明記する
   - テキストはテキストで記述し、
   - 動作確認の様子は、テキスト出力とスクリーンキャプチャを添付する
  
2 - 3 = ?
答え: -1
正解！
9 + 10 = ?
答え: 19
正解！
2 - 7 = ?
答え: 05
不正解… 正解は -5
5 * 5 = ?
答え: 25
正解！
4 + 8 = ?
答え: 12
正解！

結果: 4/5 正解

--- ログ一覧 ---
Q: 2 - 3 = ? → 正解 (-1)
Q: 9 + 10 = ? → 正解 (19)
Q: 2 - 7 = ? → 不正解 (05)
Q: 5 * 5 = ? → 正解 (25)
Q: 4 + 8 = ? → 正解 (12)
```

```

----
### **使った技法の説明**

- どこに、どんな概念・技法をなぜ・どのように適用したのか、**自分の言葉で簡潔にまとめる**

- [x]  関数の活用
time,make_log,loggerで複数の関数を定義して処理をした。
特に、make_logはログを作る工場のような役割を持っていて関数を返す関数になっている。
- [x] クロージャの活用
make_logの中で定義されたloggerは外側のlogsリストを参照してい。
- [x] 例外処理の活用 
- [x] プロトコルの活用 
- [x] クラスの定義
このコードは3っのクラスで定義されている。
questionqは問題の基本形
mathquestionはランダムな計算問題を作る
quizはクイズ全体の進行管理
- [x] インヘリタンスの定義
mathquuestionの部分でmathquestionがquestionの機能を引き継いでいる。
- [x] デコレ―タの活用
- [x] ジェネレータ、コルーチンの活用
mathqustion.make_questionの中でyieldをつかってる。
これにより問題を一度に全部作るのではなく、必要になったタイミングで問題を作っている。
- [x] 非同期処理の活用
- [x] 非ブロックI/Oの活用
- [x] メタクラスの定義、動的クラス生成
- [x] デザインパターンの活用
- [x] マルチスレッド並列処理
- [x] 自作パッケージ作成

### *考察** **自己分析**

   - **うまく行かず苦労した点、積み上げた失敗、失敗から学んだ教訓を必ず明記**する
   - 問題解決の考え方・攻略法を自分の言葉で簡潔にまとめる
   - 用いた技法、例えば、関数プログラミングの利点、プロトコルの活用場面について、応用シーンを考察する
   -  やってみて初めて分かったこと
   - 失敗の積み上げで、深く理解したこと
   - **生成AIとの適切な向き合い方**　について考察
   - この課題を通して、まだ改善できると感じた点や他の解法のアイデアを自分の言葉で論じる

今回の課題では、クイズを作るためによく使うクラスやあまり使わないクロージャやジェネレータを使って書きました。あまり使わないクロージャは苦戦しました。ログを保持する時の仕組みを理解できておらす、エラーが起きたりして何度も試行錯誤することになりました。なんでエラーなのか理解できない時もありました。この経験から、理解しないままコードを書くと必ず詰まるという基本的なことが分かった。

------

# フリープログラム

### 作品名：　ヘビゲーム




### **コード全体と、使用した技法の説明** 
   - コードを明記し、**変数・関数・クラスの役割やロジックをコメント等で簡潔に説明**する

```python
import pygame,random
import math
import time

def measure(func):
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(f"{func.__name__} 実行時間: {end - start:.4f}秒")
        return result
    return wrapper
pygame.init()
try:
    eat_sound =pygame.mixer.Sound("voice1.mp3")
except pygame.error:
    eat_sound =None
    print("音声ファイルが読み込めません")
dis=pygame.display.set_mode((500,600))

pygame.display.set_caption('ヘビゲーム')
clock =pygame.time.Clock()



#変数
blue =(30,144,255)
yellow =(255,255,0)
green =(0,255,127)
orange =(255,69,0)
purple =(148,0,211)
aqua = (0,255,255)
red = (255,0,0)
black =(0,0,0)

snake_block =10
snake_speed =15
head =green
eaten_head =purple
flash_frame =0

W , H =500,600
font_score =pygame.font.SysFont(None, 20)
font_main =pygame.font.SysFont(None,30 )


def 餌_作る():
    fx =round(random.randrange(0, W-snake_block)/snake_block)*10
    fy =round(random.randrange(0 ,H-snake_block)/snake_block)*10
    return fx,fy

def スコア_表示(score):
    s =font_score.render(f"Score :{score}",True, black)
    dis.blit(s, (10,10))

def msg_表示(message):
    text =font_main.render(message, True, red)
    dis.blit(text, (W/5,H/3))

@measure
def gameLoop():
    global eaten_head
    global snake_speed
    global flash_frame 
    game_over =False
    game_close =False
    x,y =300,300
    x1_change,y1_change =0,0
    snake_body =[(116,210), (126,210), (140,210), (154,210), (170,210)]
    蛇_長さ =len(snake_body)
    foodx ,foody =餌_作る()
    score =0

    while not game_over:
        while game_close:
            dis.fill(blue)
            スコア_表示(score)
            msg_表示("game over,press q for quit, c for restart")
            pygame.display.update()
            for event in pygame.event.get():
                if event.type ==pygame.KEYDOWN:
                    if event.key ==pygame.K_q:
                        game_over =True
                        game_close =False
                    if event.key ==pygame.K_c:
                        return gameLoop()


        for event in pygame.event.get():
            if event.type==pygame.QUIT:
                game_over=True
            if event.type ==pygame.KEYDOWN:
                if event.key ==pygame.K_LEFT:
                    x1_change = -snake_block;
                    y1_change = 0
                elif event.key ==pygame.K_RIGHT:
                    x1_change =snake_block
                    y1_change =0
                elif event.key ==pygame.K_UP:
                    x1_change =0
                    y1_change =-snake_block
                elif event.key ==pygame.K_DOWN:
                    x1_change =0
                    y1_change =snake_block
        x += x1_change
        y += y1_change

        if x<0 or  x>=W or y<0 or y>=H:
            game_close = True

        snake_body.append((x,y))
        if len(snake_body)>蛇_長さ:
            snake_body.pop(0)

        if x==foodx and y ==foody:
            foodx, foody =餌_作る()
            flash_frame =10
            snake_speed +=5
            蛇_長さ +=10
            eaten_head =purple
            score +=1
            eat_sound.play()

        if flash_frame >0:
            flash_frame -=1   
        

        dis.fill(blue)
        蛇_描く(dis,snake_body,)
        pygame.draw.rect(dis,purple,(foodx,foody, snake_block,snake_block))
        clock.tick(snake_speed)
        スコア_表示(score)
        pygame.display.update()

def 蛇_描く(画面,蛇_位置,):
    global eaten_head
    for i ,(x,y)in enumerate(蛇_位置):
        if i == len(蛇_位置) -1:
            #pygame.draw.circle(画面,eaten_head if flash_frame >0 else head,(x,y),14)
            color =eaten_head if (flash_frame >0 and flash_frame %2 ==1)else head
            pygame.draw.circle(画面,color,(x,y),12)
            pygame.draw.circle(画面,black,(x+4,y-3),4)
        else:
            pygame.draw.circle(画面,green,(x,y),12)

gameLoop()

```

### **実行結果** 
   - 実行結果の記述、**正常に動作した証跡**を明記する
   - テキストはテキストで記述し、
   - 動作確認の様子は、テキスト出力とスクリーンキャプチャを添付する
  
![alt text](<スクリーンショット 2026-01-04 174546.png>)
```

```

----
### **使った技法の説明**

- どこに、どんな概念・技法をなぜ・どのように適用したのか、**自分の言葉で簡潔にまとめる**

- [x]  関数の活用
餌、スコア、蛇やgameLoopなどのゲームを進行や関数を初期位置に戻すために使った。
- [x] クロージャの活用
- [x] 例外処理の活用 
ボイスファイルが読み込めなかった時エラーで止まらないようにするため
- [x] プロトコルの活用 
- [x] クラスの定義
- [x] インヘリタンスの定義
- [x] デコレ―タの活用
gameLoopの実行結果を計算するため
これがあればゲームが重くなった時などに原因を見つけるときに使える
- [x] ジェネレータ、コルーチンの活用
- [x] 非同期処理の活用
- [x] 非ブロックI/Oの活用
- [x] メタクラスの定義、動的クラス生成
- [x] デザインパターンの活用
- [x] マルチスレッド並列処理
- [x] 自作パッケージ作成

### *考察** **自己分析**
   - ショート・プログラムからの発展性
   - **うまく行かず苦労した点、積み上げた失敗、失敗から学んだ教訓を必ず明記**する
   - 問題解決の考え方・攻略法を自分の言葉で簡潔にまとめる
   - 用いた技法、例えば、関数プログラミングの利点、プロトコルの活用場面について、応用シーンを考察する
   -  やってみて初めて分かったこと
   - 失敗の積み上げで、深く理解したこと
   - **生成AIとの適切な向き合い方**　について考察
   - この課題を通して、まだ改善できると感じた点や他の解法のアイデアを自分の言葉で論じる

## *考察**
作るうえで苦戦したことは蛇が餌を食べたときの処理とgameLoopの処理順序です。
このゲームはへびが餌を食べると体が伸びたりスピードが上がったり効果音がならなかったなど何も起きないことがありました。
gameLoopでは入れるところを少し間違うだけで動かなくなるので何が間違っているのか全く分からず苦戦しました
あとは、えさの当たり判定がおかしいので直したい
やってみてわかったことはゲームを作るうえで関数はゲーム処理をするうえで大切だと実感しました。
gameLoopを関数にすることでゲーム全体を管理したり、ゲームオーバー後にリスタートしたとき関数を呼び出すだけでなく初期状態に戻せる。
この動作によってゲームをスムーズに処理できるようになった。


----

### 所感・今後の目標課題 (自由記述欄、フィードバック意見を尊重)

- なぜプログラミングを学ぶか
- これまで学んで得た知見
- これからの学びに活かすべき知見
- 適切な生成AIの活用方法についての見解

今後の目標は、エラーが起きたときすぐaiに頼らず自分で解決できるようになりたいです。また臨時実習までにプログラミングの基礎を完璧にしたいです。

----