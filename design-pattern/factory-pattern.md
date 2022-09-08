# Factory Methon パターン

ファクトリーパターンは、Javaで最もよく使われるデザインパターンの1つである。このタイプのデザインパターンは、作成パターンであり、オブジェクトを作成する最適な方法を提供する。

Factoryパターンでは、生成ロジックをクライアントに公開することなく、共通のインターフェースで新しく生成されたオブジェクトを指し示すことで、オブジェクトを生成する。

## はじめに

**意図**：オブジェクトを生成するためのインターフェースを定義し、そのサブクラスがどのファクトリークラスをインスタンス化するかを自ら決定するようにする。

**主解法**：インターフェース選択問題の主解法。

**使用時期**：異なる条件でインスタンスが作成される場合を明示的に想定しています。

**解決方法**: そのサブクラスにファクトリーインターフェースを実装させ、抽象的な製品も返すようにします。

**キーコード**：作成処理は、そのサブクラスで実行されます。

**応用例**：1、車が要るときは、この車がどのように作られているか、この車の中の具体的な実装を気にすることなく、工場の中から直接手に取ることができます。 2、Hibernateは、データベースだけ方言とドライバを変更する必要があることができます変更します。

**利点**：

1. 呼び出し側がオブジェクトを作成したい、その名前さえ知っていればよい。
2. 高いスケーラビリティ、あなたが製品を追加したい場合は、単にファクトリークラスを拡張することができます。 
3. 製品の特定の実装を遮蔽し、呼び出し側は、製品のインターフェイスを気にします。

**欠点**：

あなたが製品を追加するたびに、特定のクラスとオブジェクトの実装工場を追加する必要がある、システムのクラスの数は指数関数的に増加し、ある程度、システムの複雑さを高めるだけでなく、システム固有のクラスの依存性を増加させる。これは良いことではありません。

**利用シーン**：

1. ロガー：ログの記録先は、ローカルのハードディスク、システムイベント、リモートサーバーなど、ユーザーが選択可能です。 
2. データベースアクセス。システムが最終的にどのタイプのデータベースを使用するか分からない場合、また、データベースに変更が生じる可能性がある場合。 
3. サーバーに接続するためのフレームワークを設計する。「POP3」、「IMAP」、「HTTP」の3つのプロトコルが必要で、これらは一緒にインターフェースを実装する製品クラスとして使用することができる。

**注**：創造クラスパターンとして、ファクトリーメソッドパターンは、複雑なオブジェクトを生成する必要がある場合にはどこでも使用することができる。注意点としては、ファクトリーパターンは複雑なオブジェクトに適しており、単純なオブジェクト、特にnewだけで作成できるようなオブジェクトはファクトリーパターンを使う必要はない。ファクトリーパターンを使用する場合、ファクトリークラスを導入する必要があり、システムの複雑さが増します。

## 実装

Shape インターフェースと、Shape インターフェースを実装したエンティティクラスを作成します。次に、ファクトリークラスであるShapeFactoryを定義します。

FactoryPatternDemoクラスは、ShapeFactoryを使用してShapeオブジェクトを取得します。必要なオブジェクトの種類を取得するために、ShapeFactoryに情報（CIRCLE / RECTANGLE / SQUARE）を渡します。

![](https://cdn.jsdelivr.net/gh/Kakusi-IO/runoob-img/20220908110137.png)

### ステップ１

インタフェースを作成します。

```JAVA
// Shape.java
public interface Shape {
   void draw();
}
```

### ステップ２

インタフェースを実装するエンティティクラスを作成します。

```java
// Rectangle.java
public class Rectangle implements Shape {
 
   @Override
   public void draw() {
      System.out.println("Inside Rectangle::draw() method.");
   }
}
```

```java
// Square.java
public class Square implements Shape {
 
   @Override
   public void draw() {
      System.out.println("Inside Square::draw() method.");
   }
}
```

```java
// Circle.java
public class Circle implements Shape {
 
   @Override
   public void draw() {
      System.out.println("Inside Circle::draw() method.");
   }
}
```

### ステップ３

与えられた情報に基づいて、エンティティクラスのオブジェクトを生成するファクトリーを作成します。

```java
// ShapeFactory.java
public class ShapeFactory {
    
   //使用 getShape 方法获取形状类型的对象
   public Shape getShape(String shapeType){
      if(shapeType == null){
         return null;
      }        
      if(shapeType.equalsIgnoreCase("CIRCLE")){
         return new Circle();
      } else if(shapeType.equalsIgnoreCase("RECTANGLE")){
         return new Rectangle();
      } else if(shapeType.equalsIgnoreCase("SQUARE")){
         return new Square();
      }
      return null;
   }
}
```

### ステップ４

このファクトリーを使用して、型情報を渡してエンティティクラスのオブジェクトを取得します。

```java
// FactoryPatternDemo.java
public class FactoryPatternDemo {
 
   public static void main(String[] args) {
      ShapeFactory shapeFactory = new ShapeFactory();
 
      //获取 Circle 的对象，并调用它的 draw 方法
      Shape shape1 = shapeFactory.getShape("CIRCLE");
 
      //调用 Circle 的 draw 方法
      shape1.draw();
 
      //获取 Rectangle 的对象，并调用它的 draw 方法
      Shape shape2 = shapeFactory.getShape("RECTANGLE");
 
      //调用 Rectangle 的 draw 方法
      shape2.draw();
 
      //获取 Square 的对象，并调用它的 draw 方法
      Shape shape3 = shapeFactory.getShape("SQUARE");
 
      //调用 Square 的 draw 方法
      shape3.draw();
   }
}
```

### ステップ５

プログラムを実行し、次の結果を出力します。

```
Inside Circle::draw() method.
Inside Rectangle::draw() method.
Inside Square::draw() method.
```

