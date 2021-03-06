# 罗马数字转整数
[题目链接](https://leetcode-cn.com/problems/roman-to-integer/)

罗马数字包含以下七种字符: I， V， X， L，C，D 和 M。
```
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```
例如， 罗马数字 2 写做 II ，即为两个并列的 1。12 写做 XII ，即为 X + II 。 27 写做  XXVII, 即为 XX + V + II 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 IIII，而是 IV。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 IX。这个特殊的规则只适用于以下六种情况：

I 可以放在 V (5) 和 X (10) 的左边，来表示 4 和 9。

X 可以放在 L (50) 和 C (100) 的左边，来表示 40 和 90。 

C 可以放在 D (500) 和 M (1000) 的左边，来表示 400 和 900。

给定一个罗马数字，将其转换成整数。输入确保在 1 到 3999 的范围内。
```
用例 1:
输入: "III"
输出: 3

用例 2:
输入: "IV"
输出: 4

用例 3:
输入: "IX"
输出: 9

用例 4:
输入: "LVIII"
输出: 58
解释: L = 50, V= 5, III = 3.

用例 5:
输入: "MCMXCIV"
输出: 1994
解释: M = 1000, CM = 900, XC = 90, IV = 4.
```

## 我的思路

华为 OD 面试时遇到的题目: 当时的思路是,初始 `number = 0`, 找到用例中特殊的罗马数字表示, 将其找出来去除, 并把对应数值 加给 number, 剩下就都是一个罗马字符表示的数字了, 遍历相加, 就能得到转换后的结果.

别人的更好的写法:
``` JS
/**
 * @param {string} s
 * @return {number}
 */
var romanToInt = function(s) {
    let keys = ['M', 'CM', 'D', 'CD', 'C', 'XC', 'L', 'XL', 'X', 'IX', 'V', 'IV', 'I'],
        values = [1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1],
        res = 0, idx;
    while(s.length) {
        idx = keys.indexOf(s[0] + s[1]);//先判断是不是符合2个字符的单位
        if(idx >= 0){
            s = s.substring(2, s.length);
        }else{
            idx = keys.indexOf(s[0]);
            s = s.substring(1, s.length);
        }
        res += values[idx];//得到对应的值
    }
    return res;
};
```


## 别人的思路

VI =  5 + 1 = 6

IV = -1 + 5 = 4

MCMXCIV = 1000 + (-100) + 1000 + (-10) + 100 + (-1) + 4

从式子中可以看出来 从左到右遍历罗马数字时, 若左边数字小于又边数字, 则左边应该为负数.

``` JS
var romanToInt = function(s) {
    let map = {'I':1, 'V':5, 'X':10, 'L':50, 'C':100, 'D':500, 'M':1000},
        sum = 0, loop = 0, num = 0, now = 0;
    while(loop < s.length) {
        now = map[s[loop]];
        if(num < now) { // num 左边数字, now 右边数字
            sum -= num;
        } else {
            sum += num;
        }
        num = now;
        loop++;
    }
    sum += num;
    return sum;
};
```