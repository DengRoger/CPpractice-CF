# CP practicing log (codeforces) 
## first time (2022/11/06): 9:30pm ~ 11:30pm
* target : 1000~1100 (solved)
* contest 1574 problem B -> Combinatorics Homework (1100)
    * code : 
    ```c++
    #pragma GCC optimize("O3,unroll-loops")
    #include <bits/stdc++.h>
    using namespace std;
    int main(){
        ios_base::sync_with_stdio(0), cin.tie(0); 
        int m , sum , t , V[3]; 
        cin >> t ; 
        while(t--){
            int V[3] ; 
            sum = 0 ; 
            cin >> V[0] >> V[1] >> V[2] >> m ;
            sort(V,V+3) ; 
            if(m <= (V[2]+V[1]+V[0]-3) and m >= max(V[2] - V[1] - V[0] -1 ,0 )) cout << "Yes\n";
            else cout << "NO\n" ; 
        }
    }
    ```
    * point : proof the range of m ; O(1)
        * min(c - b - a - 1, 0)
            * min  -> at least 0
            * c-b-a-1 -> (2,1,0) aba , 2 - 1 = 1 
        * max = a + b + c - 3
            *  at least two char consecutive and identical can make a pair , </br> so a->b cost 1 , also b->c , c->a ; total of them are 3 ,</br> so max = a+b+c-3


* contest 1744 C -> Traffic Light (1000)
    * code:
    ```c++
    #pragma GCC optimize("O3,unroll-loops")
    #include <iostream>
    #include <algorithm>
    using namespace std;
    int main(){
        ios_base::sync_with_stdio(0), cin.tie(0);
        int t , length , nowP , gP , Max , nowM , gM ; cin >> t ; 
        char now ; 
        string Q , temp; 
        while(t--){
            Q = "x" ; temp = "" ; 
            nowP = 0 , gP = 0 ; Max = 0 , gM = 0 , nowM = 0 ; 
            cin >> length  >> now >> temp; 
            temp += temp ; Q += temp ; 
            if(now == 'g') cout << 0 << '\n' ; 
            else{
                for(int i = 1 ; i < length * 2+1 ; i++){
                    if( Q[i] == now and ( Q[i] != Q[i-1]) and (nowM == gM) ) nowP = i , nowM++ ; 
                    if( Q[i] == 'g' and ( Q[i] != Q[i-1]) and (gM == nowM-1)) gP = i , Max = max(Max , gP - nowP) , gM++ ; 
                }
                cout << Max << '\n' ; 
            }

        }    
    }
    ```
    * point : DP(sliding window) ; O(n)
        * ex. rrrgyyygy , now at y 
            * find all of pair and choose the max 
            * rrrgyyygyrrrgyyygy -> *2 because it could be a cicle 
            * first time of y at 4 , after the first time of y , first time of g at 7 </br> so , the first length = 7 - 4 , and contiune 



## second time (2022/11/07): 9:30pm ~ 11:30pm

- target : 1100~1200 (solved)
- contest 1673 B -> A Perfectly Balanced String? (1100)
    * code : 
    ```c++
    #pragma GCC optimize("Ofast")
    #include <bits/stdc++.h>
    using namespace std;
    int main(){
        ios_base::sync_with_stdio(0), cin.tie(0) ; 
        int n ; cin >> n ; string q , j; 
        for(int t = 0 ; t < n ;t++){ 
            cin >> q ; int i = 0 ; set<char> jj ; j = "" ; int judge = 1 ;
            for(i ; i < q.size() ; i++){
                auto it=jj.find(q[i]);
                if(it==jj.end()) jj.insert(q[i]) , j += q[i] ; 
                else break ; 
            }
            int jL = j.size() , jC = -1 ; 

            for(i ; i < q.size() ; i++){
                jC++;
                if(j[jC] != q[i]){
                    judge = 0 ; 
                    cout << "No\n";
                    break;
                }

                if(jC >= jL-1) jC=-1;

            }
            if(judge) cout << "Yes\n";
        }
    }
    ```
- contest 552 B -> Vanya and Books (1200)
    * code : 
    ```c++
    #pragma GCC optimize("O3,unroll-loops")
    #include"bits/stdc++.h"
    using namespace std;
    int main(){
        ios_base::sync_with_stdio(0), cin.tie(0);
        long long n,ans;cin>>n;int S=to_string(n).size();
        long long a[10]={0, 9, 189, 2889, 38889, 488889, 5888889, 68888889, 788888889, 8888888889};
        ans=a[S-1];
        ans+=(n-pow(10,S-1)+1)*S;
        cout<<ans;
    }
    ```

- contest 1681 C -> Double Sort(1200)
    * code : 
    ```c++
    #include <bits/stdc++.h>
    #define ll long long
    #define ff first
    #define ss second
    using namespace std;
    bool judge(vector<ll> a , vector<ll>b){
        for(int i = 0 ; i < a.size()-1 ; i++) if(a[i] > a[i+1] || b[i] > b[i+1]) return false;
        return true;
    }
    int main(){
        ios_base::sync_with_stdio(0), cin.tie(0);
        int t ; cin >> t ; 
        while(t--){
            int n ; cin >> n ;
            vector<ll> f(n) , s(n) ;
            vector<vector<ll>> step ; 
            for(auto&n:f) cin >> n; for(auto&n:s) cin >> n;
            for(ll i = 0 ; i < n ; i++){
                for(ll j = 0 ; j < n ; j++){
                    if(f[i] >= f[j] && s[i] >= s[j] && i < j)       swap(f[i],f[j]) , swap(s[i],s[j]) , step.push_back({j,i});
                    else if(f[i] <= f[j] && s[i] <= s[j] && i > j)  swap(f[i],f[j]) , swap(s[i],s[j]) , step.push_back({j,i});
                }
            }
            if(!judge(f,s)) cout << -1 << endl; 
            else{
                cout << step.size() << endl;
                for(auto&n:step) cout << n[0]+1 << " " << n[1]+1 << endl;
            }
        }
    }
    ```
    * point : based on bubble sort
        * u can't make sure 0~i is sorted 

## third time (2022/11/09): 9:30pm ~ 11:30pm
