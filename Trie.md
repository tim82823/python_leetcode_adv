### 字典樹
- 比較特別的樹
- root代表空字串,每個分支接續一個特定字母
- 所以單一節點代表一個字串(從root沿路經過的字母連接而成)
- 由於每層代表其中一個字母,一旦建立完成,要sort就會很快
### 適用範圍
- sort重複出現的字
- 當需要搜尋的範圍固定,卻要sort很多個單字
- 比較少次時可用BFS
- 也可以依題要求結構
### 208. implement Trie(Prefix Tree)
建立trie
- insert(),search().startswith()
- 可以用一個dict處理(也可用list).(self.tree)
- insert()
	- 使用節點ite從self.tree沿著字母往下,若對應字母無node,初始node
	- 沿途往下遷移,直到走完對應路徑(也將為初始化的初始化)
	- 由於不確定是真的有字或沿途經過,所以在尾端新增一個對應 ite["#"]="#"
- search()
	- 使用node ite從 self.tree沿著word字母往下,若對應字母無node return false
	- 一路到最尾端,if有看到"#"有出現則為True(有這個字),否則false
- startswith()
	- 使用節點ite從self.tree沿著prefix的字母往下,若對應字母無node則return false
	- 一路成功到最尾端的話 return True(表示有這組字首)
### sol:
- __ init __ (self)
	- self.tree={}
- insert (self,word:str)
	- 從word每次取c出來,以ite=self.tree開始往下走
		- 如果不在ite中,ite[c]={}(開一個新的)
		- ite=ite[c] (遞移往下)
	- 最後ite["#"]要初始化成"#"
- search(self,word:str) -> bool:
	- 同insert,但c不在ite終究return False
	- 最後 ite["#"] 是否為"#"來回傳 True/False
- startwith(self,prefix:str) -> bool:
	- 同search方法
	- 最後順利走到尾端 return True
	```python
	class Trie:

  

    def __init__(self):

        """

        Initialize your data structure here.

        """

        self.tree={}

  

    def insert(self, word: str) -> None:

        """

        Inserts a word into the trie.

        """

        ite=self.tree

        for c in word:

            if c not in ite:

                ite[c]={}

            ite=ite[c]

        ite['#']= '#'

  

    def search(self, word: str) -> bool:

        """

        Returns if the word is in the trie.

        """

        ite=self.tree

        for c in word:

            if c not in ite:

                return False

            ite=ite[c]

        return True if '#' in ite else False

  

    def startsWith(self, prefix: str) -> bool:

        """

        Returns if there is any word in the trie that starts with the given prefix.

        """

        ite=self.tree

        for c in prefix:

            if c not in ite:

                return False

            ite=ite[c]

        return True
```

