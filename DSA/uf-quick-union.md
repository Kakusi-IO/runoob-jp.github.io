# 素集合　高速合併

データに対して、素集合は主に３つの操作を提供する：

- ``unoin(p ,q)`` ｐとｑを繋ぐ
- ``find(p)`` ｐの所在集合を取得する
- ``isConnected(p ,q)`` ｐとｑは繋がっているのかを確認する

各要素をノードとみなし、その親を指し、ルートノードは自分自身を指します。下図のように、ノード3はノード2を指しているので、3と2はつながっていることになり、ノード2自体はルートノードなので自分自身を向ける。

![](https://cdn.jsdelivr.net/gh/Kakusi-IO/runoob-img/20220908111710.png)

というわけで、長さ＝１０の場合、

![](https://cdn.jsdelivr.net/gh/Kakusi-IO/runoob-img/20220908111825.png)

![](https://cdn.jsdelivr.net/gh/Kakusi-IO/runoob-img/20220908111858.png)

``unoin(4, 3)``で、４を３につなぐ：

![](https://cdn.jsdelivr.net/gh/Kakusi-IO/runoob-img/20220908111956.png)

![](https://cdn.jsdelivr.net/gh/Kakusi-IO/runoob-img/20220908112009.png)



2つの要素がつながっているかどうかは、ルートノードが同じかどうかで判断すればよい。

下図では、ノード4とノード9はともにルートノードが8であるので、4と9は繋がっている。

![](https://cdn.jsdelivr.net/gh/Kakusi-IO/runoob-img/20220908112148.png)

2つの要素を結合するには、対応するルートノードを見つけ、そのルートノードを連結させれば、連結ノードにる。

例の6と4を結ぶには、6のルートノード5を4のルートノード8に向ける。

![img](https://www.runoob.com/wp-content/uploads/2020/10/quickUnion-07.png)

## 実装

```JAVA
package runoob.union;

public class UnionFind2 {
    // parent[i]はiのparentを表す
    private int[] parent;
    private int count;

    public UnionFind2(int count){
        parent = new int[count];
        this.count = count;
        // 初期化、すべての要素は自分自身に向ける
        for( int i = 0 ; i < count ; i ++ )
            parent[i] = i;
    }

    // pのルート要素を探す
    // 時間計算量O(h)、hは木の高さ
    private int find(int p){
        assert( p >= 0 && p < count );
        // 不断去查询自己的父亲节点, 直到到达根节点
        // 根节点的特点: parent[p] == p
        while( p != parent[p] )
            p = parent[p];
        return p;
    }
    
    // pとqは繋がっているのかを確認する
    // 時間計算量O(h)、hは木の高さ
    public boolean isConnected( int p , int q ){
        return find(p) == find(q);
    }
    
    // pとqを結合
    // 時間計算量O(h)、hは木の高さ
    public void unionElements(int p, int q){
        int pRoot = find(p);
        int qRoot = find(q);
        if( pRoot == qRoot )
            return;
        parent[pRoot] = qRoot;
    }
}
```

