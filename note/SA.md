# 模擬退火法
* 與爬山演算法相同都為貪心演算法，不同處於模擬退火法加入了隨機因素。
* 先給一個高溫，讓自由度高一點(克服爬山演算法只能往下走的問題)，使程式能夠跨越小山丘找到更好的解。

![](./pic/HC.png)
設定初始解為 0，先搜尋至局部最佳解 A ，模擬退火法會以一定的概率接受往右（E）的移動，也許經過幾次這樣的不是區域性最優的移動後會到達 B 點，於是就跳出了局部（全域）最大值。

* 程式範例：`s 是值， maxGens 是最大次數，maxFails 是最多錯誤次數`
```py
def hillClimbing(s, maxGens, maxFails):   # 爬山演算法的主體函數
    print("start: ", s.str())             # 印出初始解
    fails = 0                             # 失敗次數設為 0
    # 當代數 gen<maxGen，且連續失敗次數 fails < maxFails 時，就持續嘗試尋找更好的解。
    for gens in range(maxGens):
        snew = s.neighbor()               #  取得鄰近的解
        sheight = s.height()              #  sheight=目前解的高度
        nheight = snew.height()           #  nheight=鄰近解的高度
        if (nheight >= sheight):          #  如果鄰近解比目前解更好
            print(gens, ':', snew.str())  #    印出新的解
            s = snew                      #    就移動過去
            fails = 0                     #    移動成功，將連續失敗次數歸零
        else:                             #  否則
            fails = fails + 1             #    將連續失敗次數加一
        if (fails >= maxFails):
            break
    print("solution: ", s.str())          #  印出最後找到的那個解
    return s                              #    然後傳回。
```

---

## 遺傳演算法
* 用於解決最佳化的搜尋演算法

```py
from geneticAlgorithm import GeneticAlgorithm
import random

class KeyGA(GeneticAlgorithm):
    def __init__(self, key):
        super().__init__()
        self.key = key

    def randomChromosome(self): # 隨機產生一個染色體 (一個 16 位元的 01 字串)
        bits=[]
        for _ in range(len(self.key)):
            bit = str(random.randint(0,1))
            bits.append(bit)
        return ''.join(bits)
  
    def calcFitness(self, c): # 分數是和 key 一致的位元個數
        fitness=0
        for i in range(len(self.key)):
            fitness += 1 if c[i]==self.key[i] else 0
        return fitness
  
    def crossover(self, c1, c2):
        cutIdx = random.randint(0, len(c1)-1)
        head   = c1[0:cutIdx]
        tail   = c2[cutIdx:]
        return head + tail
    
    def mutate(self, chromosome): # 突變運算
        i=random.randint(0, len(chromosome)-1) # 選擇突變點
        cMutate = chromosome[0:i]+random.choice(['0','1'])+chromosome[i+1:] # 在突變點上隨機選取 0 或 1
        return cMutate # 傳回突變後的染色體

# 執行遺傳演算法，企圖找到 key，最多執行20代，每代族群都是一百人
kga = KeyGA("10101010101010101010")
kga.run(100, 20)
```

* result:
```
============ generation 0 ===============
0 {'chromosome': '1100010101111000', 'fitness': 6}
1 {'chromosome': '1100100100010111', 'fitness': 6}
2 {'chromosome': '1111110001000001', 'fitness': 6}
3 {'chromosome': '1010010100010111', 'fitness': 6}
4 {'chromosome': '0100001111110111', 'fitness': 6}
...
95 {'chromosome': '1010101010101010', 'fitness': 16}
96 {'chromosome': '1010101010101010', 'fitness': 16}
97 {'chromosome': '1010101010101010', 'fitness': 16}
98 {'chromosome': '1010101010101010', 'fitness': 16}
99 {'chromosome': '1010101010101010', 'fitness': 16}
```