#### String_operate
- 用單引號or雙引號刮起來
- 每個單一單位為字元
- str() 轉換
- 不能跟改string本體 (string is immutable)
- slice operate
	[ start : end: step ]
- string 常用語法:
	len() - 取長
	reversed() - 反轉
	split() - 分割
	join() - 結合
	strip() - 截左右端特定字元/字串
	find() - 首次出現index
	for c in string:依序取字元
	list() - 按字元轉換串列
適用範圍
 - 先確認函式限制
 - 善用Slice
 - 含有palindrome時
 - 需要reverse字串
 - 需要按字元比對
 #### 67. Add Binary
 二進位加法
 - 只有 0 OR 1,所以只有 0+0, 1+1, 1+0, 0+1 四種結果
 - 要進位
 - 最左 OR 最右按每個個數(a[i],b[j])相加，留意進位(carry)
 - 留意位數不夠要補零，還有進位loop要繼續
 - index 要移動
 - 全部結束，反向並轉成字串
 #####  solution
 - let  res = [ ] and c(carry)='0'
- i,j分別從 len(a)-1,len(b)-1開始:
- 當i,j 還沒小於0 or c還有近位時:
	-  ai,bj=a[i],b[j] (if 位數用完則為"0")
	- if ai == bj ->將c的值附加上 res另c=ai
	- 否則res附加上的值要跟c相反
	- i,j遞減
	- 將res反轉組成 str return
```python
class Solution:

    def addBinary(self, a: str, b: str) -> str:

        res=""

        carry=0

        a,b=a[::-1],b[::-1]

        for i in range(max(len(a),len(b))):

            digitA=a[i] if i<len(a) else 0

            digitB=b[i] if i<len(b) else 0

            total=int(digitA)+int(digitB)+carry

            char=str(total%2)

            res=char+res

            carry=total//2

        if carry:

            res="1"+res

        return res
```
