- LIFO(last in first out)
	- 每次疊上去會在最上面，所以拿的時候會拿到最上面的
	- push/pop/peek()
	- python用list即可
		- push -> append()
		- pop -> pop()
		- peek -> stack[-1]
what situations need stack?
- 每次都需要檢查最後放入的東西
- need 反轉順序(且不希望一次全數反轉)
適用:
- 幾乎需要看到最後一個資料結構的題目
- 有可要搭配recursive/loop使用
	- 常使用堆疊可將遞迴解改成迭代解
### 20.vaild parenthess
合法括號
- {},[],()要左右相對
- 當碰到左括號時,可等後面碰到右括號對應
- 碰到右括號,必須要跟last one 左括對應,否則對應不起來
	- eg: ([ )] -> ")"無法跟" [ "  對應

- 往左往右一個一個看
	- 每次碰到左括號，就放入stack中
	- 每次碰到右括號,check能不能和stack last one 對應並相除
	- else present對硬不起來
### sol:
- init stack=[],dic={"{":"}","[ ":" ] ","( ":" )"}(做括號對應)
- using loop, i=0 ~ len(s)-1:
	- get s[i] value c to judgement:
	- if c is 左括號(in dic.key()) -> stack直接附加c
	- else if stack還有東西且其對應到dic的右括號跟c一樣  -> the stack pop
	- else means not found match,return false
- finish loop, if stack empty means all match, return not stack

```python
class Solution:

    def isValid(self, s: str) -> bool:

        stack=[]

        closeToOpen={"(":")","[":"]","{":"}"}

        for c in s:

            if stack and closeToOpen.get(stack[-1],'')==c:

                stack.pop()

            else:

                stack.append(c)

        return not stack
```

