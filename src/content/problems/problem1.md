---
title: "BOJ.1891"
description: "사분면"
custom_link_label: "Live Preview"
updatedDate: "Jun 6 2023"
heroImage: "/boj_logo.png"
---

```cpp

#include <iostream>
#include <string>
#include <algorithm>
#include <vector>

using namespace std;
using ll = long long;

ll d, x, y;
string input, res;

// transform quadrant information into 2D array's index
pair<ll,ll> quadrantToArr(ll row, ll col, ll cur) {

    if(cur == d) return {row, col};

    ll jump = 1LL << (d-cur-1);

    if(input[cur] == '1') col += jump;

    // idx doesn't change when it's at the 2nd quadrant.

    else if(input[cur] == '3') {
        row += jump;
    }

    else if(input[cur] == '4') {
        row += jump;
        col += jump;
    }

    return quadrantToArr(row,col,cur+1);

}

// transform 2D array's index into quadrant information
void arrToQuadrant(pair<ll,ll> idx, ll cur) {

    if(cur == d) return;

    ll jump = 1LL << (d-cur-1);
    ll row = idx.first - jump;
    ll col = idx.second - jump;

    // 1st quadrant
    if(row < 0 && col >= 0) {
        row += jump;
        res.push_back('1');
    }
    // 2nd
    else if(row < 0 && col < 0) {
        row += jump;
        col += jump;
        res.push_back('2');
    }
    // 3rd
    else if(row >= 0 && col < 0) {
        col += jump;
        res.push_back('3');
    }
    // 4th
    else if(row >= 0 && col >= 0) {
        res.push_back('4');
    }

    arrToQuadrant({row,col},cur+1);
    return;

}

void cal() {

    pair<ll,ll> idx = quadrantToArr(0,0,0);
    idx.first -= y;
    idx.second += x;
    
    // handling out of bound
    const ll MAX = 1LL << d;
    if(idx.first < 0 || idx.first >= MAX || idx.second < 0 || idx.second >= MAX) {
        res = "-1";
        return;
    }

    arrToQuadrant(idx,0);
    return;

}

int main() {

    ios_base::sync_with_stdio(false);
    cin.tie(0);

    cin >> d >> input >> x >> y;

    cal();

    cout << res;

    return 0;

}


```
