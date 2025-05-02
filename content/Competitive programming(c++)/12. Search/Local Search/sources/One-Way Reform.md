```cpp
#include <bits/stdc++.h>
using namespace std;
 
mt19937 rng(chrono::steady_clock().now().time_since_epoch().count());
int rand_int(int l, int r) {
    return uniform_int_distribution<int>(l, r)(rng);
}
void solve(){
    int n,m;
    cin>>n>>m;
    vector<pair<int,int>> edges;
    for(int i=0; i<m; i++){
        int x,y;
        cin>>x>>y;
        edges.push_back({x,y});
    }
    vector<bool> dirBest(m), dirCur(m);
    vector<int> inDeg(n+1), outDeg(n+1)/*, idb(n+1), odb(n+1)*/;
    int ans=0;
    
    int trails=1000;
    while(trails--){
        fill(inDeg.begin(), inDeg.end(),0);
        fill(outDeg.begin(), outDeg.end(),0);
        
        for(int i=0; i<m; i++){
            int a=rand_int(0, 1);
            dirCur[i]=a;
            if(a){
                outDeg[edges[i].first]++;
                inDeg[edges[i].second]++;
            }
            else{
                inDeg[edges[i].first]++;
                outDeg[edges[i].second]++;
            }
        }
        
        for(int i=0; i<m; i++){
            int ef=edges[i].first, es=edges[i].second;
            if(dirCur[i] && inDeg[ef]<outDeg[ef] && outDeg[es]<inDeg[es]){
                dirCur[i]=0;
                inDeg[ef]++, outDeg[ef]--, outDeg[es]++, inDeg[es]--;
            }
            else if((!dirCur[i]) && inDeg[ef]>outDeg[ef] && outDeg[es]>inDeg[es]){
                dirCur[i]=1;
                inDeg[ef]--, outDeg[ef]++, outDeg[es]--, inDeg[es]++;
            }
            
        }
        
        int anst=0;
        for(int i=1; i<=n; i++){
            anst+=(inDeg[i]==outDeg[i]);
        }
        if(anst>ans){
            ans=anst;
            dirBest=dirCur;
            //idb=inDeg, odb=outDeg;
        }
        
    }
    cout<<ans<<endl;
    for(int i=0; i<m; i++){
        int ef=edges[i].first, es=edges[i].second;
        if(dirBest[i]) cout<<ef<<" "<<es<<endl;
        else cout<<es<<" "<<ef<<endl;
    }
    /*
    cout<<endl;
    
    for(int i=1; i<=n; i++){
        cout<<idb[i]<<" "<<odb[i]<<endl;
    }
    */
    
}

int main() {
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    
    int t;
    cin>>t;
    while(t--){
        solve();
    }
    
    
    return 0;
}
```