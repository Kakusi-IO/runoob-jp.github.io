# 素集合　size最適化

この状態で、``union(4, 9)``を実行。

![](https://cdn.jsdelivr.net/gh/Kakusi-IO/runoob-img/20220908113046.png)

結合すると、

![](https://cdn.jsdelivr.net/gh/Kakusi-IO/runoob-img/20220908113132.png)

この構造の木はやや高くて、この時点で要素数が増えると、この方法で発生する消費量が相対的に大きくなることがわかる。この問題の解決方法は実はとても簡単で、特定のポイント操作を行う際に、まず判定を行い、要素の少ない集合のルートノードを要素の多いルートノードに向けることで、もっと低い木を高い確率で生成することができる。

```java
// コンストラクタ
public UnionFind3(int count){
    parent = new int[count];
    sz = new int[count];
    this.count = count;
    // 初期化
    for( int i = 0 ; i < count ; i ++ ){
        parent[i] = i;
        sz[i] = 1;
    }
}
```

unoin操作を行う場合、結合の方向は2つの要素が配置されている木の要素数によって決定される。

```java
public void unionElements(int p, int q){
    int pRoot = find(p);
    int qRoot = find(q);
    if( pRoot == qRoot )
        return;
    if( sz[pRoot] < sz[qRoot] ){
        parent[pRoot] = qRoot;
        sz[qRoot] += sz[pRoot];
    }
    else{
        parent[qRoot] = pRoot;
        sz[pRoot] += sz[qRoot];
    }
}
```

改良した結果は以下のようになり、9は親ノード8を指すようになる。

![](https://cdn.jsdelivr.net/gh/Kakusi-IO/runoob-img/20220908113541.png)

## 実装

```java
// UnionFind3.java
package runoob.union;

public class UnionFind3 {
    private int[] parent;
    // sz[i]はiをルートにする木の要素数
    private int[] sz;
    private int count;

    public UnionFind3(int count){
        parent = new int[count];
        sz = new int[count];
        this.count = count;
        for( int i = 0 ; i < count ; i ++ ){
            parent[i] = i;
            sz[i] = 1;
        }
    }

    private int find(int p){
        assert( p >= 0 && p < count );
        while( p != parent[p] )
            p = parent[p];
        return p;
    }

    public boolean isConnected( int p , int q ){
        return find(p) == find(q);
    }

    public void unionElements(int p, int q){
        int pRoot = find(p);
        int qRoot = find(q);
        if( pRoot == qRoot )
            return;
        // 誰の誰につなぐのを要素数で決定
        if( sz[pRoot] < sz[qRoot] ){
            parent[pRoot] = qRoot;
            sz[qRoot] += sz[pRoot];
        }
        else{
            parent[qRoot] = pRoot;
            sz[pRoot] += sz[qRoot];
        }
    }
}
```

