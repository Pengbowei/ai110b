
此程式[來源](https://gist.github.com/namoshizun/7a3b820b013f8e367e84c70b45af7c34)


## 定義玩家猜拳基本參數 
* *utilities*：<br>
定義行為「玩家1」; 列為「玩家2」<br>
遊戲中勝利值為 **1** ; 敗值為 **-1** <br>
遊戲狀態(P1 = ROCK ; P2 = PAPER) 調用`utilities.loc['ROCK', 'PAPER']`。
```py
class RPS:
    actions = ['ROCK', 'PAPER', 'SCISSORS']
    n_actions = 3
    utilities = pd.DataFrame([
        # ROCK  PAPER  SCISSORS
        [ 0,    -1,    1], # ROCK
        [ 1,     0,   -1], # PAPER
        [-1,     1,    0]  # SCISSORS
    ], columns=actions, index=actions)

```

* **regret_sum**：每次迭代後遺憾值總和。
* **avg_strategy**：平衡策略使其收斂達納什均衡。
* **strategy_sum**, **regret_sum** 定義為 4 x 3 Array。
```py
class Player:
    def __init__(self, name):
        self.strategy, self.avg_strategy,\
        self.strategy_sum, self.regret_sum = np.zeros((4, RPS.n_actions))
        self.name = name
```

## 賽局進行
* 開始
```py
if __name__ == '__main__':
    game = Game()

    print ('==== Use simple regret-matching strategy === ')
    game.play()
    print ('==== Use averaged regret-matching strategy === ')
    game.conclude()
    game.play(avg_regret_matching=True)
```

* 賽局定義兩位玩家P1、P2：「Alasdair」、「Calum」。
* 進行10000局猜拳。
* [更新策略](#更新策略) -> [決策](#決策) -> [依據賽局結果更新遺憾值](#更新遺憾值)
```py
class Game:
    def __init__(self, max_game=10000):
        self.p1 = Player('Alasdair')
        self.p2 = Player('Calum')
        self.max_game = max_game

    def play(self, avg_regret_matching=False):
            def play_regret_matching():
                for i in range(0, self.max_game):
                    self.p1.update_strategy()
                    self.p2.update_strategy()
                    a1 = self.p1.action()
                    a2 = self.p2.action()
                    self.p1.regret(a1, a2)
                    self.p2.regret(a2, a1)

                    winner = self.winner(a1, a2)
                    num_wins[winner] += 1
    
    num_wins = {
        self.p1: 0,
        self.p2: 0,
        'Draw': 0
    }

    play_regret_matching() 
    if not avg_regret_matching else play_avg_regret_matching()
    print (num_wins)
```

## 策略更新
* 從累積的遺憾值計算遺憾匹配策略。
* 儲存計算後策略機率，接下來會根據策略機率做決策。
```py
def update_strategy(self):
        """
        set the preference (strategy) of choosing an action to be proportional to positive regrets
        e.g, a strategy that prefers PAPER can be [0.2, 0.6, 0.2]
        """
        self.strategy = np.copy(self.regret_sum)
        self.strategy[self.strategy < 0] = 0  # reset negative regrets to zero

        summation = sum(self.strategy)
        if summation > 0:
            # normalise
            self.strategy /= summation
        else:
            # uniform distribution to reduce exploitability
            self.strategy = np.repeat(1 / RPS.n_actions, RPS.n_actions)

        self.strategy_sum += self.strategy

```

## 決策
* 依照更新完後的策略機率對賽局做出決策。
```py
def action(self, use_avg=False):
        """
        select an action according to strategy probabilities
        """
        strategy = self.avg_strategy if use_avg else self.strategy
        return np.random.choice(RPS.actions, p=strategy)
```

## 更新遺憾值
* 計算玩家的遺憾並加入累積的遺憾總和中。
```py
def regret(self, my_action, opp_action):
        """
        we here define the regret of not having chosen an action as the difference between the utility of that action
        and the utility of the action we actually chose, with respect to the fixed choices of the other player.
        compute the regret and add it to regret sum.
        """
        result = RPS.utilities.loc[my_action, opp_action]
        facts = RPS.utilities.loc[:, opp_action].values
        regret = facts - result
        self.regret_sum += regret
```
