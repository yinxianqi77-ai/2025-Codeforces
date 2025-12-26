## A. Blackslex and Password
https://codeforces.com/contest/2179/problem/A  
tags:math,strings,observation

解法：考虑 $s_{i}-s_{x+i}-\dots-s_{mx+i}$ 这个序列，由于里面的每个字母都不同，所以里面至多 $k$ 个字母，所以 $m$ 的最大值是 $k-1$ 。  
当密码无法被构造时，必定存在一个长度为 $n+1$ 的序列，这在 $n=kx+1$ 时就已经出现（ $i=1$ 的序列），也就得到本题答案。
```python
for _ in range(int(input())):
    k,x = map(int,input().split())
    print(k*x+1)
```

## B. Blackslex and Showering
https://codeforces.com/contest/2179/problem/B 以及 http://cs101.openjudge.cn/cs1012025feclass11/M30442/    
tags:dp,greedy,implementation

解法：直接模拟过程即可，注意左右的边界条件。
```python
for _ in range(int(input())):
    n = int(input())
    arr = list(map(int,input().split()))
    ans = 0
    MAX = abs(arr[1]-arr[0]) # 左边界
    for i in range(1,n):
        if i == n-1:
            MAX = max(MAX,abs(arr[i]-arr[i-1])) # 右边界
        else:
            MAX = max(MAX,abs(arr[i]-arr[i-1])+abs(arr[i+1]-arr[i])-abs(arr[i+1]-arr[i-1]))
        ans += abs(arr[i]-arr[i-1])
    print(ans-MAX)
```

## C. Blackslex and Number Theory
https://codeforces.com/contest/2179/problem/C   
tags:implementation,math,number theory,sortings,observation

解法：只要发现对于数组中很大的那些数，x 的选取是很轻松的，就能解决这个问题。设排序后数组为 $arr$ 。首先，答案 $k$ 必须小于 $arr[1]$，因为 $arr[0] = arr[1] (\mathrm{mod} \,k)$，此时我们发现 $k$ 只可能等于 $arr[0]$ 或 $arr[1]-arr[0]$ ，取较大者即可。
```python
for _ in range(int(input())):
    n = int(input())
    arr = sorted(list(map(int,input().split())))
    print(max(arr[0],arr[1]-arr[0]))
```

## D. Blackslex and Penguin Civilization
https://codeforces.com/contest/2179/problem/D   
tags:bitmasks,constructive algorithms,greedy,math,recursion

解法：假设我们已经得到使得 $S_{n-1}(p)$ 最优的数组 $f_{n-1}$ （长度为 $2^{n-1}$ ），现在考虑使 $S_{n}(p)$ 最优。首先，末位为 $1$ 的数有 $2^{n-1}$ 个，并且与剩下的位数独立，所以最优的安排方式是将这些数安排在前面，剩下 $n-1$ 位的摆放方式要最优，就是按照 $f_{n-1}$ 排列，所以最佳数组 $f_{n}$ 是：   

$$
f_{n} = [2f_{n-1}+1] + \mathrm{list}(\mathrm{range}(0,2^n,2))
$$
```python
perm = [[1,0]]
for i in range(16):
    perm.append([p*2+1 for p in perm[-1]]+list(range(0,2**(i+2),2)))
for _ in range(int(input())):
    n = int(input())
    print(*perm[n-1])
```

## E. Blackslex and Girls
https://codeforces.com/contest/2179/problem/E    
tags:constructive algorithms,geometry,math

第一道硬题来了！    
解法：先给出一个试探解：将奇数的 $p_{i}$ 分解为 $\frac{p_{i}+1}{2}$ 和  $\frac{p_{i}-1}{2}$ ，将偶数的 $p_{i}$ 分解为 $\frac{p_{i}}{2}-1$ 和 $\frac{p_{i}}{2}+1$ ，分别根据 $s$ 分配给 $a$ 数组和 $b$ 数组。接下来，分四种情况讨论：
1. $\sum a_{i}>x,\sum b_{i}>y$ ，此时无论如何也不可能满足题目要求，输出no。
2. $\sum a_{i}\leq x,\sum b_{i}\leq y$ ，此时 $a$ 数组和 $b$ 数组分别可以在 $s_{i}=0$ 和 $s_{i} = 1$ 处添加 $a_{i}$ 和 $b_{i}$ ，使得总和达到 $x$ 和 $y$ ，输出yes。但是：注意 $s$ 可以等于 $0$ 或 $\mathrm{bin}(2^n-1)$ ，这样的情况下可能不存在 $s_{i}=0$ 或 $s_{i} = 1$ 的 $i$ ，需要单独讨论。
	1. $s=0$ ，那么我们需要统计 $p_{i}$ 为偶数的个数，将这些下标 $i$ 对应的 $b_{i}$ 加一（仍然满足 $a_{i}>b_{i}$，而 $p_{i}$ 为奇数时则无法依此操作），就得到了 $b$ 数组的最大可能和，再与 $y$ 进行比较即可。
	2. $s=\mathrm{bin}(2^n-1)$ ，那么我们需要统计 $p_{i}$ 为偶数的个数，将这些下标 $i$ 对应的 $a_{i}$ 加一（仍然满足 $a_{i}<b_{i}$，而 $p_{i}$ 为奇数时则无法依此操作），就得到了 $a$ 数组的最大可能和，再与 $x$ 进行比较即可。
3.  $\sum a_{i}>x,\sum b_{i}\leq y$ ，此时我们可以对于 $s_{i}=1$ 的下标，适当减去 $a$ 数组中的一些数，并同等的增加 $b$ 数组对应的数，使得 $a_{i}+b_{i}\geq p_{i}$ 始终成立。操作之后，需要满足两条判别条件：1. 我们可以记录所有 $a_{i}<b_{i}$ 的 $a_{i}$ 总和，$a$ 数组的总和至多可以减去这么多，将这些数减去之后 $a$ 数组总和不能超过 $x$。2. $b$ 数组总和增加之后不能超过 $y$。如果满足这两条，就将问题转化为第二种情形，并且由先前操作可知 $s$ 中必定既有 $0$ 又有 $1$ ，不需要讨论，直接输出yes。如果不满足，直接输出no。
4.   $\sum a_{i}\leq x,\sum b_{i}> y$ ，与情况3类似，这里不再赘述。
```python
for _ in range(int(input())):
    n,x,y = map(int,input().split())
    s = input()
    p = list(map(int,input().split()))
    a,b = [],[]
    even = 0
    movea,moveb = 0,0
    for i in range(n):
        if p[i]%2 == 0:
            even += 1# 和为偶数的个数
            if int(s[i]) == 0:
                a.append(p[i]//2+1)
                b.append(p[i]//2-1)
                moveb += p[i]//2-1
            else:
                b.append(p[i]//2+1)
                a.append(p[i]//2-1)
                movea += p[i]//2-1
        else:
            if int(s[i]) == 0:
                a.append(p[i]//2+1)
                b.append(p[i]//2)
                moveb += p[i]//2
            else:
                b.append(p[i]//2+1)
                a.append(p[i]//2)
                movea += p[i]//2
    sa,sb = sum(a),sum(b) # a数组和b数组总和
    if sa>x and sb>y:
        print("NO")               
    elif sa>x and sb <= y:
        if sa-x<=y-sb and movea >= sa-x:
            print("YES")
        else:
            print("NO")
    elif sa<=x and sb>y:
        if sb-y<=x-sa and moveb >= sb-y:
            print("YES")
        else:
            print("NO")
    elif s != "0"*n and s!="1"*n:
        print("YES")
    elif int(s) == 0:
        if even+sb+x-sa >= y:
            print("YES")
        else:
            print("NO")
    else:
        if even+sa+y-sb >= x:
            print("YES")
        else:
            print("NO")
```

## F. Blackslex and Another RGB Walking
https://codeforces.com/problemset/problem/2179/F   
tags:constructive algorithms,graphs,interactive,number theory,bfs

由于每个节点只能着一种颜色，并且要表征与节点 $1$ 的距离信息，容易想到按照到节点1的距离除以3的余数对所有的节点着色。
```python
from collections import deque
use = "rgb"
mode = input()
if mode == "first":
    for _ in range(int(input())):
        n,m = map(int,input().split())
        visited = [0]*n
        dis = [0]*n
        edge = {}
        color = [""]*n
        for i in range(m):
            ai,bi = map(int,input().split())
            if ai > bi:
                ai,bi = bi,ai
            if ai in edge:
                edge[ai].append(bi)
            else:
                edge[ai] = [bi]
            if bi in edge:
                edge[bi].append(ai)
            else:
                edge[bi] = [ai]
        q = deque()
        q.append(1)
        step = 0
        while q:
            l = len(q)
            for __ in range(l):
                node = q.popleft()
                visited[node-1] = 1
                dis[node-1] = step
                color[node-1] = use[step%3]
                for next_n in edge[node]:
                    if visited[next_n-1] == 0:
                        q.append(next_n)
                        visited[next_n-1] = 1
            step += 1
        print(*color,sep = "")
```

在查询时，每一个点 $v$ 的周围**至多有两种颜色的点**。   
反证法：如果有三种颜色的点，那么里面必定有一个点 $u$ 使得 $\mathrm{dis}(1,v) = \mathrm{dis}(1,u)$ ，考察由 $1,v,u$ 三个点组成的最小环，里面有奇数个点，与二分图条件矛盾。  
所以只需分两种情况讨论： 
1. 只有一种颜色相邻，说明目前所在点是距离极大值点，向任意一个点走即可。
2. 有两种颜色相邻，这里举例说明：如果相邻的颜色是 $r$ 和 $b$ ，由于相邻节点的距离差1，说明自己所在节点的颜色为 $g$ ，进而说明应该前往颜色为 $r$ 的点。其他颜色可以类比。
```python
else:
    for _ in range(int(input())):
        for __ in range(int(input())):
            l = int(input())
            s = input()
            if "r" in s and "g" in s:
                print(s.index("g")+1)
            elif "g" in s and "b" in s:
                print(s.index("b")+1)
            elif "b" in s and "r" in s:
                print(s.index("r")+1)
            elif "r" in s:
                print(s.index("r")+1)
            elif "g" in s:
                print(s.index("g")+1)
            elif "b" in s:
                print(s.index("b")+1)
```

## G. Blackslex and Penguin Migration(Challenge)
https://codeforces.com/contest/2179/problem/G   
（在challenge版本中，你最多只能查询 $3n^{2}$ 次。）  
tags:brute force,interactive,math

为了确定所有点的相对位置，我们需要找到一些点的绝对位置。注意到如果从任意一个点开始查询，距离它最远的点一定在角上，所以可以先选定一个点 $A$，查询 $n^{2}-1$ 次，得到一个确定在角上的点 $B$。  
光有一个位置确定的点是不够的，我们至少还需要一个点，注意到两个相邻的角 $B,C$ 的距离为 $n-1,$并且当点 $A$ 不是一个角时，角上的点 $C$ 相较于其他所有距离为 $n-1$ 的点，它到点 $A$ 的距离都更大，所以可以确定点 $C$。当 $A$ 是一个角（最大距离为 $2n-2$ ）时就更简单了，对于所有到点 $A$ 距离为 $n-1$ 的点，随机选择一个点，测量它到其他点的距离，距离最大的就一定在角上。   
最后再测一下所有点到 $C$ 的距离就可以唯一确定其他点的位置了。 
```python
for _ in range(int(input())):
    n = int(input())
    ans = [[0]*n for _ in range(n)]
    disa = {1:0}
    maxd = 0
    cand = []
    for i in range(2,n**2+1):
        print(f"? 1 {i}")
        disa[i] = int(input())
        if disa[i] == n-1:
            cand.append(i)
        if maxd < disa[i]:
            maxd = disa[i]
            c1 = i
    if maxd == 2*n-2:
        c1 = 1
        maxd = 0
        idx = 0
        start = cand[0]
        for i in range(1,n):
            print(f"? {start} {cand[i]}")
            d = int(input())
            if d > maxd:
                maxd = d
                idx = i
        c2 = cand[idx]
        dic = disa
    else:
        cand = []
        disb = {c1:0}
        for i in range(1,n**2+1):
            print(f"? {c1} {i}")
            disb[i] = int(input())
            if disb[i] == n-1:
                cand.append(i)
        maxd = 0
        for c in cand:
            if disa[c] > maxd:
                maxd = disa[c]
                c2 = c
        dic = disb
    disc = {}
    for i in range(1,n**2+1):
        print(f"? {c2} {i}")
        disc[i] = int(input())
        x = (dic[i]+disc[i]-(n-1))//2
        y = dic[i] - x
        ans[x][y] = i
    print("!")
    for _ in range(n):
        print(*ans[_])
```
冷知识：你可以在每一个样例的Output中查看自己程序的query次数。
## H. Blackslex and Plants
https://codeforces.com/contest/2179/problem/H   
tags:bitmasks,prefix sum,dp,math,difference

考虑一个简单版本的问题，当 $f(x)=x$ 时如何解决这个问题？  
我们发现，对于数组的二阶差分数组，原来的数组的每一个区间变化都只对应二阶差分数组中 $3$ 个值 的变化，例如：对于 $n=5$ ，第一个区间为 $[1,5]$ ，原数组，一阶差分和二阶差分的变化过程如下（前后有两个辅助0）：
原数组： 

$$
0\quad 0\quad 1\quad 2\quad 3\quad 4\quad 5\quad 0\quad 0
$$

一阶差分：  

$$
0\quad 1\quad 1\quad 1\quad 1\quad 1\quad -5\quad 0
$$

二阶差分：  

$$
1\quad 0\quad 0\quad 0\quad 0\quad -6\quad 5
$$

故可以在 $O(n+m)$ 的时间复杂度内解决。
回到本题，观察 $f(x)$ 可以发现它可以拆分成 $\lceil \log_{2}n \rceil$ 个等差数列，第 $i$ 个数列的位置间隔为 $2^i$ ，公差为 $2^i(2^i-2^{i-1})$，首项为0，于是可以对每一个等差数列如法炮制。 具体而言，对于位置间隔为 $k$ 的等差数列，我们可以建立 $k$ 个二阶差分数组，根据初始位置的不同在对应的数组上做变化。
时间复杂度 $O((n+m)\log n)$
```python
from math import ceil
for _ in range(int(input())):
    n,q = map(int,input().split())
    query = [list(map(int,input().split())) for _ in range(q)]
    k = len(bin(n)) - 2 # 等价于ceil(log_2 n)
    start = 0
    water = [0]*n
    for i in range(k):
        step = 2**i # 等差数列的位置间隔
        delta = step-start # 每一个等差数列的公差
        start = 2**i
        dif2 = [[0]*(n//step+3) for _ in range(step)]
        for l,r in query:
            if r-l+1 < step: # 如果区间间隔太小以至于不存在第 i 个等差数列
                continue
            first = l + step-1
            m = (first-1)%step # 用初始位置的余数分类到对应二阶差分数组
            dl = ceil(first/step) # 差分数组处对应的起点
            dr = (r-first)//step+dl # 差分数组处对应的终点
            # 二阶差分数组只会有三个位置的数变化
            dif2[m][dl-1] += delta*step
            dif2[m][dr] += -delta*(dr-dl+2)*step
            dif2[m][dr+1] += delta*(dr-dl+1)*step
        # 重构一阶差分数组（前后各有一个0）
        dif1 = [[0] for _ in range(step)]
        for j in range(step):
            for a in range(n//step+3):
                dif1[j].append(dif1[j][-1]+dif2[j][a])
        # 重构原数组（前后各有两个0）
        new = [[0] for _ in range(step)]
        for j in range(step):
            for a in range(n//step+4):
                new[j].append(new[j][-1]+dif1[j][a])
        # 将所有原数组变成一个
        for a in range(2,n//step+3):
            for j in range(step):
                if j+(a-2)*step<n:
                    water[j+(a-2)*step] += new[j][a]
    print(*water)
```
