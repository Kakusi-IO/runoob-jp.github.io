# プロキシパターン

Proxyパターンでは、あるクラスが他のクラスの機能を代弁する。このタイプのデザインパターンは、構造パターンである。

Proxyパターンでは、既存のオブジェクトを利用してオブジェクトを作成し、外部との機能的なインタフェースを提供する。

## 紹介

**意図**：他のオブジェクトがこのオブジェクトへのアクセスを制御するためのプロキシを提供すること。

**主に解決する問題**：アクセスするオブジェクトがリモートマシンにある場合など、オブジェクトに直接アクセスしづらい問題。オブジェクト指向システムにおいて、あるオブジェクトに直接アクセスすると、何らかの理由でユーザやシステム構造に問題が生じることがあり（例えば、オブジェクト作成のオーバーヘッドが大きい、ある操作に対してセキュリティ制御が必要、プロセス外アクセスが必要など）、このオブジェクトにアクセスする際にアクセス層を追加したい。

**使用する場合**：クラスへのアクセスをある程度制御したい場合。

**解決方法**：中間層を追加する。

**キーコード**：プロキシされたクラスと組み合わせて実装する。

**応用例**：

1. Windowsのショートカット。
2. 鉄道の切符を買うのは、必ずしも駅で買うのではなく、リセラーポイントに行くことも可能です。 
3. 小切手や預金通帳は、口座にある資金の代理人です。小切手は、市場取引において現金の代わりに使用され、発行者の口座で資金を管理するものです。 
4. Spring AOP

**メリット**：

1. 責任の所在が明確。
2. 高い拡張性 
3. インテリジェント

**デメリット**：

1. プロキシパターンの種類によっては、クライアントと実サブジェクトの間にプロキシオブジェクトが追加されるため、リクエストの処理速度が低下する場合がある。 
2. プロキシパターンを実装するために追加作業が必要であり、プロキシパターンの中には実装が非常に複雑なものがある。

**利用シーン**：責任を持って、通常次のような利用シーンがある。

1. リモートプロキシ
2. 仮想プロキシ。 
3. コピーオンライトプロキシ。 
4. Protect or Access proxy　保護プロキシ。 
5. キャッシュプロキシ。 
6. ファイアウォールプロキシ。 
7. シンクロナイズ（Synchronization）エージェント。 
8. Smart Reference（スマートリファレンス）プロキシ。

**注**：

1. Adapter パターンとの違い：Adapter パターンは主に、プロキシパターンは、プロキシクラスのインターフェイスを変更することはできませんが、検討対象のオブジェクトのインターフェイスを変更します。 
2. Decorator パターンとの違い：Decorator パターンの順序で機能を強化するために、プロキシパターンは、制御することです。

## 実装

Imageインターフェースと、それを実装したエンティティクラスを作成します。ProxyImageは、RealImageオブジェクトを読み込む際のメモリ使用量を削減するためのプロキシクラスです。

ProxyPatternDemoクラスは、ProxyImageを使用して、読み込むべきImageオブジェクトを取得し、要求に応じて表示します。

![](D:\Users\Iehana\Documents\Books\runoob-jp\design-pattern\img\20211025-proxy.svg)

### ステップ１

インタフェースを作成します。

```java
// Image.java
public interface Image {
   void display();
}
```

### ステップ２

インタフェースを実装するエンティティクラスを作成します。

```java
// RealImage.java
public class RealImage implements Image {
 
   private String fileName;
 
   public RealImage(String fileName){
      this.fileName = fileName;
      loadFromDisk(fileName);
   }
 
   @Override
   public void display() {
      System.out.println("Displaying " + fileName);
   }
 
   private void loadFromDisk(String fileName){
      System.out.println("Loading " + fileName);
   }
}
```

```java
// ProxyImage.java
public class ProxyImage implements Image{
 
   private RealImage realImage;
   private String fileName;
 
   public ProxyImage(String fileName){
      this.fileName = fileName;
   }
 
   @Override
   public void display() {
      if(realImage == null){
         realImage = new RealImage(fileName);
      }
      realImage.display();
   }
}
```

### ステップ３

ProxyImage を使用して、要求に応じて RealImage クラスのオブジェクトを取得します。

```java
// ProxyPatternDemo.java
public class ProxyPatternDemo {
   
   public static void main(String[] args) {
      Image image = new ProxyImage("test_10mb.jpg");
 
      // １回目は画像をディスクから読み込む
      image.display(); 
      System.out.println("");
      // ２回目以降は読み込み不要
      image.display();  
   }
}
```

### ステップ４

プログラムを実行し、次の結果を出力します。

```
Loading test_10mb.jpg
Displaying test_10mb.jpg

Displaying test_10mb.jpg
```

