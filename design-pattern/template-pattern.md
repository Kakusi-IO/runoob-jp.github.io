# テンプレートパターン

Template Patternでは、抽象クラスがそのメソッドを実行する方法・テンプレートをあからさまに定義する。そのサブクラスは、必要に応じてメソッドの実装をオーバーライドすることができるが、呼び出しは抽象クラスで定義されたのと同じ方法で行われる。このタイプのデザインパターンは、振る舞いパターンである。

## 紹介

**意図**：アルゴリズムのフレームワークを操作で定義し、各操作の実装をサブクラスに委ねる。テンプレートメソッドにより、サブクラスはアルゴリズムの構造を変えることなく、特定のステップを再定義することができます。

**主に解決する問題**：一部のメソッドは汎用的であるにもかかわらず、すべてのサブクラスで書き直されている。

**使用する場合**：汎用的な方法がたくさんある。

**解決方法**：これらの汎用的なアルゴリズムを抽象化して取り出す。

**キーコード**：汎用的なメソッドを抽象クラスで実装し、他のステップはサブクラスで実装する。

**メリット**：

1. 変わらない部分をカプセル化し、変わる部分をサブクラスに委ねる。
2. 公開コードを抽出し、メンテナンスが容易である。
3. 動作は親クラスが制御し、子クラスが実装する。

**デメリット**：実装が異なるごとにサブクラスが必要になるため、クラス数が増えて、システムが膨大しすぎる。

**シナリオ**：

1. 複数のサブクラス共通メソッドがあり、ロジックも同じ。
2. 重要な、それで複雑なメソッドは、テンプレートメソッドとして定義することができる。

**注**：悪意のある操作を防ぐため、一般的にテンプレートメソッドはfinalキーワードに付加される。

## 実装

操作を定義した抽象クラスGameを作成し、テンプレートメソッドをfinalにしてオーバーライドされないようにする。CricketとFootballはGameを継承したエンティティクラスで、抽象クラスのメソッドをオーバーライドする。

TemplatePatternDemoというデモクラスでは、Gameを使ってTemplateパターンを操作確認する。

![](https://raw.githubusercontent.com/Kakusi-IO/runoob-img/main/20220928161508.png)

### ステップ１

抽象クラスを定義し、そのテンプレートメソッドをfinalで修飾する。

```java
// Game.java
public abstract class Game {
   abstract void initialize();
   abstract void startPlay();
   abstract void endPlay();
 
   //テンプレート
   public final void play(){
 
      initialize();
 
      startPlay();
 
      endPlay();
   }
}
```

### ステップ２

エンティティクラスを作成する。

```java
// Cricket.java
public class Cricket extends Game {
 
   @Override
   void endPlay() {
      System.out.println("Cricket Game Finished!");
   }
 
   @Override
   void initialize() {
      System.out.println("Cricket Game Initialized! Start playing.");
   }
 
   @Override
   void startPlay() {
      System.out.println("Cricket Game Started. Enjoy the game!");
   }
}
```

```java
// Football.java
public class Football extends Game {
 
   @Override
   void endPlay() {
      System.out.println("Football Game Finished!");
   }
 
   @Override
   void initialize() {
      System.out.println("Football Game Initialized! Start playing.");
   }
 
   @Override
   void startPlay() {
      System.out.println("Football Game Started. Enjoy the game!");
   }
}
```

### ステップ３

テンプレート方法を呼び出して、操作確認する。

```java
// TemplatePatternDemo
public class TemplatePatternDemo {
   public static void main(String[] args) {
 
      Game game = new Cricket();
      game.play();
      System.out.println();
      game = new Football();
      game.play();      
   }
}
```

### ステップ４

プログラムを実行し、次の結果を出力する。

```
Cricket Game Initialized! Start playing.
Cricket Game Started. Enjoy the game!
Cricket Game Finished!

Football Game Initialized! Start playing.
Football Game Started. Enjoy the game!
Football Game Finished!
```

