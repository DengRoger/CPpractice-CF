# Algorithm 
- [segment tree](#segmentTree)
    * 前言
    * segTree基本結構
        * Struct segTree code 
        * query
    * lazy tag
        * lazy tag code  
    * 動態開點
    * 特殊優化
        * Sparse Table 
            * code (Sparse Table) 

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
        各位有發現這時間複雜度變低很多嗎，其實線段樹也是相同的概念做出來的！
        但這次是把DP表改成一棵樹
* segment tree 結構：
    ![](https://i.imgur.com/YuEqJ4I.png)
    *   原理
        由[0,5]做二元樹把他們的區間分開，然後在遞迴回來的時候去紀錄value
        建樹時間複雜度為 O(n*log(n)) , 獲取區間為 O(log(n)) #每次二分搜去找點。
    *   Struct segTree code 
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
        * 每次去二分搜找區間 並組合 
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
        
*   特殊優化
    *   提要
        **不知道看到這邊 有沒有人發現 當cases = n^2 , 但是只有n個點得時候 DP的解法比較快
        線段樹建樹要O(nlogn) + 找點 O(n^2(log(n))) 但DP只要 建表O(n^2) + 找對O(n^2)
        所以可以加一個前置判斷 當 尋找次數 > 點的個數^2 就直接改用DP來做優化。**
        但應該幾乎沒有題目會出這種case , 因為他給的空間要超過 x 
        ```
        建表log(n) + 尋找((x^2)*log(n)) = ((x^2)+1)*log(x) 
        x => ((x^2)+1)*log(x) = 2*10^9 
        x = 14450 
        ```
        所以如果他是 2D DP表 記憶體空間必須給超過208802500 才有可能 基本上可以斷定他在競賽中不可優化了
        但有個資料結構剛好可以補全這部分的問題 : Sparse Table (感謝louisfghbvc的建議xD)

    *   Sparse Table
        <preprocess , query , update an element>
        稀疏表 <O(nlgn) , O(1)  , O(nlgn)>
        線段樹 <O(n)    , O(lgn), O(lgn) >
    
<h2 id="convexHull">convexHull</h2>
