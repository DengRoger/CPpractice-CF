# Algorithm 
[![hackmd-github-sync-badge](https://hackmd.io/kImG7w0vRJOIOhv2DBA6xQ/badge)](https://hackmd.io/kImG7w0vRJOIOhv2DBA6xQ)

- [segment tree](#segmentTree)
    * 前言
    * segTree基本結構
        * StructSEG
        * query
        * update node
        * lazy tag
    * 動態開點
        
- [BIT](#BIT)

- [convex Hull](#convexHull)


<h2 id="segmentTree">segmentTree</h2>

*   前言 (記憶體換時間)
    segment tree的作用：以低時間複雜度 解決(區間,交點)問題
    * 我們先嘗試以傳統的方式解決區間問題
        給定一個數列 n => [1 , 2 , 3 , 5 , -1 , 3 , 0] 並給定 n^2組index=>( i , j ) 
        試問 每組的n[i] ~ n[j] 最小數值為多少 ？ 
        假設我們不做任何優化 每次都需把 n[i]~n[j]掃一遍 掃t組 time complexity = O(n^3)
        ![](https://i.imgur.com/jLnGCK1.png)

    *   那嘗試一下DP的思維 我們花O(n^2)的時間把它建成表  
        我們知道min(n[0] ~ n[0])=1，那我們可由min(n[0] ~ n[0])推測 min(n[0] ~ n[1]) = 1
        所以只要是相鄰的格子都只需要O(1)時間複雜度 去獲得區間最小值。
        ![](https://i.imgur.com/8Vw9tkh.png)
        
        其實線段樹的精髓也在記錄，但這次是把DP表改成一棵樹
* segment tree 結構：
    ![](https://i.imgur.com/YuEqJ4I.png)
    *   原理(StructSEG)
        由[0,5]做二元樹把他們的區間分開，然後在遞迴回來的時候去紀錄value
        建樹時間複雜度為 O(n) , 獲取區間為 O(log(n))
        *   Struct segTree code timeComplexity = O(n) 
        ```c++
        void StructSEG(int l , int r , ll node){
            if(l==r) seg[node] = v[r] ; 
            else{
                int mid = (l+r) >> 1 , tmp = node << 1;
                StructSEG(l,mid,tmp) ; 
                StructSEG(mid+1,r,tmp+1) ;
                seg[node] = seg[tmp] + seg[tmp+1] ; 
            }
        }
        ```
    * query
        * 每次去二分搜找區間 並組合 timeComplexity = log(n) 
        * query code
        ```c++
        int query(int tL , int tR , int nL , int nR , int node){
            int mid = (nL+nR)>>1 , ans = 0 ; 
            if(tL <= nL && nR <= tR) return seg[node] ;
            if(tL <= mid) ans += query(tL,tR,nL   ,mid,(node<<1)  );
            if(tR  > mid) ans += query(tL,tR,mid+1,nR ,(node<<1)+1);  
            return ans ; 
        }
        ```
    * updateNode
        * 更新節點 timeComplexity = log(n)
        * code 
        ```c++
        void updateNode(int idx , int val , int l , int r , int node){
            if(l == r){
                seg[node] = val ;
                v[idx] = val ; 
            }
            else{
                int m = (l + r) >> 1 ; 
                int leftNode  = (node << 1) ; 
                int rightNode = (node << 1) + 1 ; 
                if(idx <= m && idx >= l) updateNode(idx , val , l , m , leftNode ) ;
                else updateNode(idx , val , m+1,r , rightNode ) ; 
                seg[node] = seg[leftNode] + seg[rightNode] ; 
            } 
        }
        ```
        * lazy tag 
          簡介：即使updateNode 也不需要即時全部走過 除非你要走這條路徑時才會將node更新

        **當cases = 點^2 , DP的解法會快一些
        線段樹建樹要O(nlogn) + 找點 O(n^2(log(n))) 但DP只要 建表O(n^2) + 找對O(n^2)
        所以可以加一個前置判斷 當 尋找次數 > 點的個數^2 就直接改用DP來做優化。**
        但不太可能拿來優化 , 除非空間給超過 x 
        ```
        建表log(n) + 尋找((x^2)*log(n)) = ((x^2)+1)*log(x) 
        x => ((x^2)+1)*log(x) = 2*10^9 
        x = 14450 
        ```
        記憶體空間必須給超過208802500 ，基本上可以斷定他在競賽中不可優化了
        ■ ︿ ■

<h2 id="BIT">BIT</h2>

<h2 id="convexHull">convexHull</h2>
