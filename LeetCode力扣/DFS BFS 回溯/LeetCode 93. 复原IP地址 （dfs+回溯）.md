## 题目
给定一个只包含数字的字符串，复原它并返回所有可能的 IP 地址格式。

有效的 IP 地址 正好由四个整数（每个整数位于 0 到 255 之间组成，且不能含有前导 0），整数之间用 '.' 分隔。

例如："0.1.2.201" 和 "192.168.1.1" 是 有效的 IP 地址，但是 "0.011.255.245"、"192.168.1.312" 和 "192.168@1.1" 是 无效的 IP 地址。

 
示例 1：

```javascript
输入：s = "25525511135"
输出：["255.255.11.135","255.255.111.35"]
```

示例 2：

```javascript
输入：s = "0000"
输出：["0.0.0.0"]
```

示例 3：

```javascript
输入：s = "1111"
输出：["1.1.1.1"]
```

示例 4：

```javascript
输入：s = "010010"
输出：["0.10.0.10","0.100.1.0"]
```

示例 5：

```javascript
输入：s = "101023"
输出：["1.0.10.23","1.0.102.3","10.1.0.23","10.10.2.3","101.0.2.3"]
```

 

提示：

```javascript
0 <= s.length <= 3000
s 仅由数字组成
```

## 解题思路
做第一步时我们有几种 选择 ？以 "25525511135" 为例：
选 "2" 作为第一个片段
选 "25" 作为第一个片段
选 "255" 作为第一个片段
可以选择切出三种长度，接下来，切第二个片段，又面临三种选择。
这会像一棵树一样向下分支，我们用 DFS 去遍历所有选择，并且是回溯，为什么是回溯？
因为某一步的选择可能来到一个错误的状态，得不到正确的结果，不要往下做了，撤销最后一个选择，回到选择前的状态，去试另一个选择。

dfs 主要看剪枝条件，如下：
- 当长度为4，但是没有用完整个字符串时，剪掉
- 每一层判断时，我们可以选择3个片段，长度为1的，长度为2的，还有长度为3的，假如加上某个片段后，导致超过了原字符串的长度，剪掉
- 如果每一层取的片段长度大于1并且存在前导0，剪掉
- 如果当前取的片段长度等于3，但是超过了255，剪掉

最后，留下树上还有的结果，然后继续dfs+回溯

## 代码

```javascript
var restoreIpAddresses = function(s) {
    let res = []
    let dfs = (subRes,start) => {
      if(subRes.length===4 && start === s.length){
        res.push(subRes.join('.'))
      }
      if(subRes.length===4 && start!= s.length) return
      for(let len=1;len<=3;len++){
        if(start+len-1>=s.length) return
        if(len>1 && s[start]=='0') return
        let str = s.substring(start,start+len)
        if(len===3 && (str-'0')>255) return
        subRes.push(str)
        dfs(subRes,start+len)
        subRes.pop()
      }
    } 
    dfs([],0)
    return res
};
```
