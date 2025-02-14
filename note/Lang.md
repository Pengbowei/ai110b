# 人工智慧第十二週筆記
## 自然語言
* [自然語言](https://gitlab.com/ccc109/ai/-/blob/master/10-lang/rule/01-basic/01-%E8%AA%9E%E8%A8%80%E8%99%95%E7%90%86%E7%B0%A1%E4%BB%8B.md)
* 由歷史過程衍生出來的語言稱為「自然語言」，常見有: 中文、英文、日文等各國語言。
* 非刻意設計出來的語言，而由某人創造設計出來的語言稱為人造語言，而前述兩者之結合稱為標記語言。
* 在資訊工程的領域是需要面對及研究的課題。
## 人造語言
* 人造語言的種類很多，但大部分都是程式類的語言，像是高階語言 (像是 C、Ruby、Python)、組合語言 (像是 x86、ARM、CPU0 的組合語言)、還有高階語言在翻譯成組合語言之前通常會經過某種中介語言等等。
![pic1](./pic/pic1.png)
## 語言的處理
* 編譯器可以很容易將高階語言轉換成組合語言或機器碼。
* 然而對於自然語言的處理而言，目前的技術顯得相當力不從心，與人類的翻譯水平還有一些落差。
## 語法理論
* [語法理論](https://gitlab.com/ccc109/ai/-/blob/master/10-lang/rule/01-basic/02-%E8%AA%9E%E6%B3%95%E7%90%86%E8%AB%96.md)
* 不管是程式語言或自然語言，都可以用語法進行描述。但不同的是，程式語言的語法是設計者自行定義的，所以凡是不符合語法的程式都會直接被視為錯誤的程式。而在處理自然語言的時候，凡是文章中的句子，不管合不合語法，你的程式都要能夠處理。這是自然語言處理之所以困難的原因之一。
## 語言的層次
* 語言處理可分為以下幾個層次
1. 詞彙掃描: 詞彙層次
2. 語法解析: 語句層次
3. 語意解析: 文章層次
4. 語言合成: 回應階段，將詞彙組成語句、再將語句組合成文章呈現出來 
![pic2](./pic/pic2.png)
* 一個翻譯系統需具備整合以上4項之功能
## Chomsky Hierarchy (喬姆斯基語言階層）
![pic3](./pic/pic3.png)
* Type1 語言的語法有點限制，因為每個規則的左邊至少要有一個非終端項目 A，但其前後可以連接任意規則，這種語法所能描述的語言稱為「對上下文敏感的語言」 (Context-Sensitive)，因為 可以決定之後到底是否要接 ，所以前後文之間是有關係的，因此才叫做「對上下文敏感的語言」。這種語言在計算理論上可以對應到「線性有界的非決定性圖靈機」，也就是一台「記憶體有限的電腦」。
* Type2 語言的語法限制更大，因為規則左邊只能有一個非終端項目 (以 A 代表)，規則右邊則沒有限制這種語言被稱為「上下文無關的語言」(Context Free) ，在計算理論上可以對應到 「非決定性的堆疊機」(non-deterministic pushdown automaton)。
* Type3 的語法限制是最多的，其規則的左右兩邊都最多只能有一個非終端項目 (以 A, B 表示) ，而且右端的終端項目 (以 a 表示) 只能放在非終端項目 B 的前面。這種語言稱為「正規式」(Regular)，可以用程式設計中常用的「正規表達式」(Regular Expression) 表示，對應到計算理論中的有限狀態機(Finite State Automaton)。
![pic4](./pic/pic4.png)
* Type2 所不能處理的語言當中，有個最著名的範例是 anbncna_nb_nc_nan​bn​cn​ ，由於這當中 abc 三個字母必須按照順序各出現 n 次，而 Type2 的與上下文無關語法，無法記憶到底已經產生了幾個 ，所以也就無法產生出這樣的語言了。
![pic5](./pic/pic5.png)
## 語句結構
* [生成語法](https://gitlab.com/ccc109/ai/-/blob/master/10-lang/rule/01-basic/03-%E7%94%9F%E6%88%90%E8%AA%9E%E6%B3%95.md)
```
S = NP VP
NP = DET N
VP = V NP
N = dog | cat
V = chase | eat
DET = a | the
```
## 運算式語法
![pic6](./pic/pic6.png)
## BNF 與生成語法
BNF語法 | 生成語言
-------- |--------
```S = N V ```<br>```N = John ｜ Mary```<br>```V = eats ｜ talks``` | 	L = {John eats, John talks, Mary eats, Mary Talks}
---
![pic7](./pic/pic7.png)
## 語法樹
![pic8](./pic/pic8.png)
## 語言處裡技術
* [語言處裡技術](https://www.slideshare.net/ccckmit/ss-15898210)
## jieba
* (base) peng@pengbaiweis-MacBook-Pro 08-jieba % pip install jieba
## opencc
* (base) peng@pengbaiweis-MacBook-Pro 09-opencc % pip install opencc-python-reimplemented
## code
* e2c.py
```py
import sys
e2c = { 'dog':'狗', 'cat':'貓', 'a': '一隻', 'the': '這隻', 'that':'那隻', 'chase':'追', 'eat':'吃' }

def mt(elist): 
    clist = []
    for e in elist:
        c = e2c[e]
        clist.append(c)
    return clist

c = mt(sys.argv[1:])
print(c)
```
* 執行結果
```
(base) peng@pengbaiweis-MacBook-Pro 02-lookup % python e2c.py a dog
['一隻', '狗']
(base) peng@pengbaiweis-MacBook-Pro 02-lookup % python e2c.py a dog chase a cat
['一隻', '狗', '追', '一隻', '貓']
```
* gen1.py
```py
import random as r

'''
S = NP VP
NP = DET N
VP = V NP
N = dog | cat
V = chase | eat
DET = a | the
'''

def S(p):
    p['p'] = 1.0
    return NP(p) + VP(p)

def NP(p):
    return DET(p) + N(p)

def VP(p):
    return V(p) + NP(p)

def N(p):
    p['p'] = p['p'] * 0.5
    return r.choices(['dog', 'cat'], [0.3, 0.7])

def V(p):
    p['p'] = p['p'] * 0.5
    return r.choices(['chase', 'eat'], [0.6, 0.4])

def DET(p):
    p['p'] = p['p'] * 0.5
    return r.choices(['a', 'the'], [0.5, 0.5])

p = {'p': 1.0}

for _ in range(10):
    print('%s %f'%(S(p), p['p']))
```
* 執行結果
```
(base) peng@pengbaiweis-MacBook-Pro 03-gen % python gen1.py
['the', 'cat', 'chase', 'a', 'cat'] 0.031250
['the', 'cat', 'eat', 'the', 'cat'] 0.031250
['a', 'dog', 'eat', 'a', 'cat'] 0.031250
['a', 'cat', 'chase', 'the', 'cat'] 0.031250
['a', 'dog', 'chase', 'the', 'dog'] 0.031250
['the', 'cat', 'chase', 'the', 'cat'] 0.031250
['a', 'cat', 'chase', 'the', 'cat'] 0.031250
['the', 'dog', 'chase', 'a', 'dog'] 0.031250
['a', 'cat', 'eat', 'a', 'cat'] 0.031250
['the', 'cat', 'chase', 'a', 'dog'] 0.031250
```
* gen_english1.py
    * 生成語句 每次都會不同
```py
import random as r

'''
S = NP VP
NP = DET N
VP = V NP
N = dog | cat
V = chase | eat
DET = a | the
'''

def S():
    return NP() + ' ' + VP()

def NP():
    return DET() + ' ' + N()

def VP():
    return V() + ' ' + NP()

def N():
    return r.choice(['dog', 'cat'])

def V():
    return r.choice(['chase', 'eat'])

def DET():
    return r.choice(['a', 'the'])

print(S())

```
* 執行結果
```
(base) peng@pengbaiweis-MacBook-Pro 03-gen % python gen_english1.py
the dog chase a cat
(base) peng@pengbaiweis-MacBook-Pro 03-gen % python gen_english1.py
a dog eat the cat
(base) peng@pengbaiweis-MacBook-Pro 03-gen % python gen_english1.py
a dog eat the cat
(base) peng@pengbaiweis-MacBook-Pro 03-gen % python gen_english1.py
a dog chase a dog
```
* gen_chinese1.py
```py
import random as r

'''
S = NP VP
NP = DET N
VP = V NP
N = 狗 | 貓
V = 追 | 吃
DET = 一隻 | 這隻
'''

def S():
    return NP() + ' ' + VP()

def NP():
    return DET() + ' ' + N()

def VP():
    return V() + ' ' + NP()

def N():
    return r.choice(['狗', '貓'])

def V():
    return r.choice(['追', '吃'])

def DET():
    return r.choice(['一隻', '這隻'])

print(S())
```
* 執行結果
```
(base) peng@pengbaiweis-MacBook-Pro 03-gen % python gen_chinese1.py
這隻 貓 吃 一隻 狗
(base) peng@pengbaiweis-MacBook-Pro 03-gen % python gen_chinese1.py
這隻 狗 吃 一隻 狗
(base) peng@pengbaiweis-MacBook-Pro 03-gen % python gen_chinese1.py
一隻 狗 追 這隻 狗
```
* gen_chinese2.py
```py
import random as r

'''
S => NP VP	          句子 = 名詞子句 接 動詞子句
NP => Det Adj* N PP*  名詞子句 = 定詞 接 名詞 接副詞子句
VP => V NP       	  動詞子句 = 動詞 接 名詞子句
PP => P NP	          副詞子句 = 副詞 接 名詞子句
N = 狗 | 貓
V = 追 | 吃
DET = 一隻 | 這隻
Adj = 白 | 黑 | 兇 | 帥
P = 那個 | 有 
'''

def S():
    return NP() + ' ' + VP()

def star(NT):
	list = []
	while r.choice([0,1])==0:
		list.append(NT())
	return ' '.join(list)

# NP => Det Adj* N                // PP*
def NP():
    return DET() + ' ' + star(ADJ) + ' ' + N() # + ' ' + star(PP)

def VP():
    return V() + ' ' + NP()

# PP => P NP	          副詞子句 = 副詞 接 名詞子句
def PP():
	return P() + ' ' + NP()

def P():
    return r.choice(['那個', '有'])

def N():
    return r.choice(['狗', '貓'])

def V():
    return r.choice(['追', '吃'])

def DET():
    return r.choice(['一隻', '這隻'])

# Adj = 白 | 黑 | 兇 | 可愛的
def ADJ():
	return r.choice(['白', '黑', '兇', '帥'])

print(S())

```
* 執行結果
```
(base) peng@pengbaiweis-MacBook-Pro 03-gen % python gen_chinese2.py
這隻  貓 吃 這隻  狗
(base) peng@pengbaiweis-MacBook-Pro 03-gen % python gen_chinese2.py
這隻 黑 白 兇 白 帥 貓 吃 一隻  貓
(base) peng@pengbaiweis-MacBook-Pro 03-gen % python gen_chinese2.py
一隻 白 帥 兇 狗 吃 這隻  狗
(base) peng@pengbaiweis-MacBook-Pro 03-gen % python gen_chinese2.py
這隻 帥 狗 吃 一隻 兇 黑 狗
(base) peng@pengbaiweis-MacBook-Pro 03-gen % python gen_chinese2.py
這隻 白 白 帥 白 白 黑 狗 追 這隻 帥 白 兇 帥 白 狗
```
* gen_exp1.py
```py
import random as r

'''
E = N | E [+/-*] E
N = 0-9
'''

def E():
	gen = r.choice(["N", "EE"])
	# print('gen=', gen)
	if gen == "N":
		return N()
	else:
		return E() + r.choice(["+", "-", "*", "/"]) + E()

def N():
	return r.choice(["1", "2", "3", "4", "5", "6", "7", "8", "9"])

e = E()
print(e, "=", eval(e))
```
* 執行結果
```
(base) peng@pengbaiweis-MacBook-Pro 03-gen % python gen_exp1.py
5 = 5
(base) peng@pengbaiweis-MacBook-Pro 03-gen % python gen_exp1.py
9 = 9
(base) peng@pengbaiweis-MacBook-Pro 03-gen % python gen_exp1.py
4 = 4
(base) peng@pengbaiweis-MacBook-Pro 03-gen % python gen_exp1.py
8 = 8
(base) peng@pengbaiweis-MacBook-Pro 03-gen % python gen_exp1.py
3 = 3
```
* gen_exp2_bug.py
```py
import random as r

'''
E = N | E [+/-*] E
N = 0-9 | (E)
'''

def E():
	gen = r.choice(["N", "EE"])
	# print('gen=', gen)
	if gen == "N":
		return N()
	else:
		return E() + r.choice(["+", "-", "*", "/"]) + E()

def N():
    rnd = r.random()
    if rnd > 0.2:
	    return r.choice(["1", "2", "3", "4", "5", "6", "7", "8", "9"])
    else:
        return "("+E()+")"

e = E()
print(e, "=", eval(e))
```
* 執行結果
```
(base) peng@pengbaiweis-MacBook-Pro 03-gen % python gen_exp2_bug.py
5 = 5
(base) peng@pengbaiweis-MacBook-Pro 03-gen % python gen_exp2_bug.py
4*5+(6) = 26
(base) peng@pengbaiweis-MacBook-Pro 03-gen % python gen_exp2_bug.py
2 = 2
(base) peng@pengbaiweis-MacBook-Pro 03-gen % python gen_exp2_bug.py
7 = 7
(base) peng@pengbaiweis-MacBook-Pro 03-gen % python gen_exp2_bug.py
6 = 6
``` 
* gen_math.py
```py
import random

peoples = ["小明", "小華", "小莉", "大雄"]
objs = ["蘋果", "橘子", "柳丁", "番茄"]

def People():
    return random.choice(peoples)

def Object():
    return random.choice(objs)

owner = People()
obj = Object()
nOwn = random.randint(3, 20)

def MathTest():
  return "問題:\t"+Own()+"\n\t"+Give()+"\n\t又"+Give()+"\n\t"+Question()

def Own():
    return owner+"有"+str(nOwn)+"個"+obj

def Give():
    global nOwn
    nGive = random.randint(1, nOwn)
    nOwn-=nGive
    return "給了"+People()+str(nGive)+"個"

def Question():
    return "請問"+owner+"還有幾個"+obj+"?"

peoples.remove(owner)
print(MathTest())
print("\n答案:\t"+str(nOwn)+"個")
```
* 執行結果
```
(base) peng@pengbaiweis-MacBook-Pro 05-mathnlp % python gen_math.py
問題:   大雄有15個柳丁       
        給了小莉10個
        又給了小明1個        
        請問大雄還有幾個柳丁?

答案:   4個
```
* textgen.py
```py
#!/usr/bin/python
# -*- coding: UTF-8 -*-

import os, re
import random,readJSON

data = readJSON.读JSON文件("data.json")
名人名言 = data["famous"] # a 代表前面垫话，b代表后面垫话
前面垫话 = data["before"] # 在名人名言前面弄点废话
后面垫话 = data['after']  # 在名人名言后面弄点废话
废话 = data['bosh'] # 代表文章主要废话来源

xx = "学生会退会"

重复度 = 2

def 洗牌遍历(列表):
    global 重复度
    池 = list(列表) * 重复度
    while True:
        random.shuffle(池)
        for 元素 in 池:
            yield 元素

下一句废话 = 洗牌遍历(废话)
下一句名人名言 = 洗牌遍历(名人名言)

def 来点名人名言():
    global 下一句名人名言
    xx = next(下一句名人名言)
    xx = xx.replace(  "a",random.choice(前面垫话) )
    xx = xx.replace(  "b",random.choice(后面垫话) )
    return xx

def 另起一段():
    xx = ". "
    xx += "\r\n"
    xx += "    "
    return xx

if __name__ == "__main__":
    xx = input("请输入文章主题:")
    for x in xx:
        tmp = str()
        while ( len(tmp) < 6000 ) :
            分支 = random.randint(0,100)
            if 分支 < 5:
                tmp += 另起一段()
            elif 分支 < 20 :
                tmp += 来点名人名言()
            else:
                tmp += next(下一句废话)
        tmp = tmp.replace("x",xx)
        print(tmp)
```
* 執行結果
```
(base) peng@pengbaiweis-MacBook-Pro 04-textgen % python textgen.py > gen1.txt
人生的價值
``` 
* parse_math.py
```py
from math_dict import tagMap

words="小明 有 5 個 蘋果 ， 給 了 小華 3 個 蘋果 ， 請問 他 還 剩 幾 個 蘋果 ？".split(" ")
print(words)
wi = 0

def isTag(tag):
    tagWords=tagMap[tag]
    if tagWords == None: 
        return False
    else:
        return words[wi] in tagWords

def next(tag):
    global wi
    print("tag="+tag+" word="+words[wi])
    if isTag(tag):
        word = words[wi]
        wi += 1
        return word
    
    raise Error("Error !")

def T():
    while wi < len(words):
        S()

# S=Q? NP? v? V v? NP* .
def S():
    if isTag("Q"):
        next("Q")
    while not isTag("V") and not isTag("v"):
        NP()
    if isTag("v"):
        next("v")
    next("V")
    if isTag("v"):
        next("v")
    while not isTag("."):
        NP()  
    next(".")

# NP = (D d)? N
def NP():
    if (isTag("D")):
        next("D")
        next("d")
    
    next("N")

T()
```
* 執行結果
```
(base) peng@pengbaiweis-MacBook-Pro 05-mathnlp % python parse_math.py
['小明', '有', '5', '個', '蘋果', '，', '給', '了', '小華', '3', '個', '蘋果', '，', '請問', '他', '還', '剩', '幾', '個', '蘋果', '？']
tag=N word=小明
tag=V word=有  
tag=D word=5   
tag=d word=個  
tag=N word=蘋果
tag=. word=，  
tag=V word=給  
tag=v word=了  
tag=N word=小華
tag=D word=3   
tag=d word=個  
tag=N word=蘋果
tag=. word=，  
tag=Q word=請問
tag=N word=他
tag=v word=還
tag=V word=剩
tag=D word=幾
tag=d word=個
tag=N word=蘋果
tag=. word=？
```
* eliza.py
```py
'''
以下為本程式回答問題時使用的 Q&A 規則，例如對於以下 Q&A 規則物件

: 'Q':"想 | 希望", 'A':"為何想*呢?|真的想*?|那就去做阿?為何不呢?",

代表的是，當您輸入的字串中有「想」或「希望」這樣的詞彙時，
程式就會從 'A': 欄位中的回答裏隨機選出一個來回答。

回答語句中的 * 代表比對詞彙之後的字串，舉例而言、假如您說：

    我想去巴黎

那麼我們的程式從這四個可能的規則中隨機挑出一個來產生答案，產生的答案可能是：

為何想去巴黎呢?
真的想去巴黎?
那就去做阿?
為何不呢?

Eliza 就是一個這麼簡單的程式而已。
'''

import re
import math
import random as R
# Q&A 陣列宣告
qa_list = [
{ 'Q':"謝謝", 'A':"不客氣!" },
{ 'Q':"對不起 | 抱歉 | 不好意思", 'A':"別說抱歉 !|別客氣，儘管說 !" },
{ 'Q':"可否 | 可不可以", 'A':"你確定想*?" },
{ 'Q':"我想", 'A':"你為何想*?" },
{ 'Q':"我要", 'A':"你為何要*?" },
{ 'Q':"你是", 'A':"你認為我是*?" },
{ 'Q':"認為 | 以為", 'A':"為何說*?" },
{ 'Q':"感覺", 'A':"常有這種感覺嗎?" },
{ 'Q':"為何不", 'A':"你希望我*!" },
{ 'Q':"是否", 'A':"為何想知道是否*?" },
{ 'Q':"不能", 'A':"為何不能*?|你試過了嗎?|或許你現在能*了呢?" },
{ 'Q':"我是", 'A':"你好，久仰久仰!" },
{ 'Q':"甚麼 | 什麼 | 何時 | 誰 | 哪裡 | 如何 | 為何 | 因何", 'A':"為何這樣問?|為何你對這問題有興趣?|你認為答案是甚麼呢?|你認為如何呢?|你常問這類問題嗎?|這真的是你想知道的嗎?|為何不問問別人?|你曾有過類似的問題嗎?|你問這問題的原因是甚麼呢?" },
{ 'Q':"原因", 'A':"這是真正的原因嗎?|還有其他原因嗎?" }, 
{ 'Q':"理由", 'A':"這說明了甚麼呢?|還有其他理由嗎?" },
{ 'Q':"你好 | 嗨 | 您好", 'A':"你好，有甚麼問題嗎?" },
{ 'Q':"或許", 'A':"你好像不太確定?" },
{ 'Q':"不曉得 | 不知道", 'A':"為何不知道?|在想想看，有沒有甚麼可能性?" },
{ 'Q':"不想 | 不希望", 'A':"有沒有甚麼辦法呢?|為何不想*呢?|那你希望怎樣呢?" }, 
{ 'Q':"想 | 希望", 'A':"為何想*呢?|真的想*?|那就去做阿?為何不呢?" },
{ 'Q':"不", 'A':"為何不*?|所以你不*?" },
{ 'Q':"請", 'A':"我該如何*呢?|你想要我*嗎?" },
{ 'Q':"你", 'A':"你真的是在說我嗎?|別說我了，談談你吧!|為何這麼關心我*?|不要再說我了，談談你吧!|你自己*" },
{ 'Q':"總是 | 常常", 'A':"能不能具體說明呢?|何時?" },
{ 'Q':"像", 'A':"有多像?|哪裡像?" },
{ 'Q':"對", 'A':"你確定嗎?|我了解!" },
{ 'Q':"朋友", 'A':"多告訴我一些有關他的事吧!|你認識他多久了呢?" },
{ 'Q':"電腦", 'A':"你說的電腦是指我嗎?" }, 
{ 'Q':"難過", 'A':"別想它了|別難過|別想那麼多了|事情總是會解決的"},
{ 'Q':"高興", 'A':"不錯ㄚ|太棒了|這樣很好ㄚ"},
{ 'Q':"是阿|是的", 'A':"甚麼事呢?|我可以幫助你嗎?|我希望我能幫得上忙!" },
{ 'Q':"", 'A':"我了解|我能理解|還有問題嗎 ?|請繼續說下去|可以說的更詳細一點嗎?|這樣喔! 我知道!|然後呢? 發生甚麼事?|再來呢? 可以多說一些嗎|接下來呢? |可以多告訴我一些嗎?|多談談有關你的事，好嗎?|想多聊一聊嗎|可否多告訴我一些呢?" }
]

def answer(say):
	for qa in qa_list: # 對於每一個 QA
		qList = qa['Q'].split("|") # 取出 Q 部分，分割成一個一個的問題字串 q
		aList = qa['A'].split("|") # 取出回答 A 部分，分割成一個一個的回答字串 q
		for q in qList: # 對於每個問題字串 q
			if q.strip() == "": # 如果是最後一個「空字串」的話，那就不用比對，直接任選一個回答。
				return R.choice(aList) # 那就從答案中任選一個回答
			m = re.search("(.*)"+q.strip()+"([^?.;]*)", say)
			if m: # 比對成功的話
				tail = m.group(2) # 就取出句尾
				# 將問句句尾的「我」改成「你」，「你」改成「我」。
				tail = tail.replace("我", "#").replace("你", "我").replace("#", "你")
				return R.choice(aList).replace('*', tail) # 然後將 * 改為句尾進行回答
	return "然後呢？" # 如果發生任何錯誤，就回答「然後呢？」來混過去。


def eliza():
	print('你好，我是 Eliza ! ')
	while (True):
		say = input('> ') # 取得使用者輸入的問句。
		if say == 'bye':
			break
		ans = answer(say)
		print(ans)

eliza()

```
* 執行結果
```
(base) peng@pengbaiweis-MacBook-Pro 06-eliza % python eliza.py
你好，我是 Eliza ! 
> 你好 
你好，有甚麼問題嗎?
> 請問你是誰
你認為我是誰?
``` 
* jieba1.py
```py
# 斷詞示例

import jieba

seg_list = jieba.cut("我来到北京清华大学", cut_all=False)
print("Default Mode: " + "/ ".join(seg_list))  # 精确模式

#輸出 Default Mode: 我/ 来到/ 北京/ 清华大学
```
* 執行結果
```
(base) peng@pengbaiweis-MacBook-Pro 08-jieba % python jieba1.py
Building prefix dict from the default dictionary ...
Dumping model to file cache C:\Users\柯泓吉\AppData\Local\Temp\jieba.cache
Loading model cost 0.888 seconds.
Prefix dict has been built successfully.
Default Mode: 我/ 来到/ 北京/ 清华大学
``` 
* jieba2.py
```py
import sys
import jieba

seg_list = jieba.cut(sys.argv[1], cut_all=False)
print("斷詞結果: " + "/ ".join(seg_list))
```
* 執行結果
```
(base) peng@pengbaiweis-MacBook-Pro 08-jieba % python jieba2.py 一隻狗追一隻貓
Building prefix dict from the default dictionary ...
Dumping model to file cache /var/folders/n6/pwk4c27x4_vdqfntd2mx2_gc0000gn/T/jieba.cache
Loading model cost 0.537 seconds.
Prefix dict has been built successfully.
斷詞結果: 一/ 隻/ 狗/ 追/ 一/ 隻/ 貓
```
