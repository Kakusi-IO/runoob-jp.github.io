# 素集合　rank最適化

先はサイズに基づく最適化を紹介しましたが、以下に示すように、``union(4, 2)``のように、問題になるケースもある。

![](https://cdn.jsdelivr.net/gh/Kakusi-IO/runoob-img/20220908114054.png)

前のアルゴリズムによると、サイズの最適化では、要素の少ない木のルートノードは、要素の多い木ノードを指す。操作後、このように高さが1つ多い4になります。

![](https://cdn.jsdelivr.net/gh/Kakusi-IO/runoob-img/20220908114222.png)

このように、木の大きさに頼って指向を決定するのは全く正しくなく、このように、2つの木の高さに基づいて、低いルートノードを高いルートノードにポインティングするという、ランクに基づく最適化を具体的に決定する方がより正確であることがわかる。

![img](https://www.runoob.com/wp-content/uploads/2020/10/rank-03.png)

rank配列を用意して、iをルートとする集合が表現する木の高さを示す。

```java
...
private int[] rank;   // iをルートとする木の高さ
private int[] parent; 
private int count;    
...
```

2つの要素を結合する場合、ルートノードの高さを比較する必要があり、全体の処理は、木の高さをhとすると、O(h)の時間計算量である。

```java
...
public void unionElements(int p, int q){
    int pRoot = find(p);
    int qRoot = find(q);
    if( pRoot == qRoot )
        return;

    if( rank[pRoot] < rank[qRoot] ){
        parent[pRoot] = qRoot;
    }
    else if( rank[qRoot] < rank[pRoot]){
        parent[qRoot] = pRoot;
    }
    else{ // rank[pRoot] == rank[qRoot]
        parent[pRoot] = qRoot;
        rank[qRoot] += 1; // 高さは一つ増加
    }
}
...
```

## 実装

```JAVA
// UnionFind4.java
package runoob.union;

public class UnionFind4 {
    private int[] rank;   
    private int[] parent; 
    private int count;    

    public UnionFind4(int count){
        rank = new int[count];
        parent = new int[count];
        this.count = count;
        for( int i = 0 ; i < count ; i ++ ){
            parent[i] = i;
            rank[i] = 1;
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
        if( rank[pRoot] < rank[qRoot] ){
            parent[pRoot] = qRoot;
        }
        else if( rank[qRoot] < rank[pRoot]){
            parent[qRoot] = pRoot;
        }
        else{ // rank[pRoot] == rank[qRoot]
            parent[pRoot] = qRoot;
            rank[qRoot] += 1; 
        }
    }
}
```

