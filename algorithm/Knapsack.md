# 💫Knapsack algorithm(dp)

`dp[i][j]`

> 처음부터 i번째까지의 물건을 살피고, 배낭의 용량이 j일 때 배낭에 넣을 수 있는 가치의 합

`dp[i][j]`라는 점화식을 세우기 위해서는 i번째의 물건을 넣는 경우와, 넣지 않는 경우 이 2가지의 경우를 고려해야 합니다. 
$$
dp[i][j] = dp[i-1][j]
$$

$$
dp[i][j] = max(dp[i-1][j], dp[i-1][j-w[i]] + v[i])
$$

간단하게 말해서 i번째 물건을 넣었을 때와 넣지 않았을 때, 이 2가지의 경우 중 더 큰 값을 가져오는 식으로 dp배열을 만들어 나가면 되는 문제이다.

```javascript
for(let i=1;i<=N;i++){
	for(let j=1;j<=K;j++){
		if(j-w[i]>=0){          // i번째 물건을 넣기 위해서는 현재 배낭의 용량(j)에서 i의 무게를 뺐을 때 0 이상이어야 한다.
			dp[i][j] = Math.max(dp[i-1][j], dp[i-1][j-w[i]] + v[i])
		}
		else{                  // i번째 물건을 담을 수 없는 경우에는 i번째 물건을 담지 않는 경우만 선택할 수 있다.
			dp[i][j] = dp[i-1][j]
		}
	}
}

console.log(dp[N][K])
```



이 알고리즘의 시간복잡도는 dp 테이블을 구성하는 과정에 비례한다. 때문에 O(NK)가 기본적으로 필요하고, i번째 물건을 담는 경우와 담지 않는 경우 딱 2가지 경우만이 존재하기 때문에 상수 시간 만큼의 배가 이루어지고 이는 O(NK)라는 기존 시간복잡도에 영향을 주지 않는다.

<br/>

이 냅색 알고리즘의 가장 기본적인 문제는 백준 [12865](https://www.acmicpc.net/problem/12865)번 문제이다. 이 문제에서 위의 코드를 통해 dp 테이블을 구성시키게 되면 다음과 같은 dp 테이블을 얻을 수 있다.

| (index) |  0   |  1   |  2   |  3   |  4   |  5   |  6   |  7   |
| :-----: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
|    0    |  0   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |
|    1    |  0   |  0   |  0   |  0   |  0   |  0   |  13  |  13  |
|    2    |  0   |  0   |  0   |  0   |  8   |  8   |  13  |  13  |
|    3    |  0   |  0   |  0   |  6   |  8   |  8   |  13  |  14  |
|    4    |  0   |  0   |  0   |  6   |  8   |  12  |  13  |  14  |

<br/>

---

하지만, 위의 테이블을 채우다보면 바로 한 칸 윗줄만을 계속해서 참고하는 것을 알 수 있다. 이를 통해, 이차원 배열이 아닌 일차원 배열을 사용하여 이 문제를 풀 수 있다.

배낭의 크기 만큼의 dp 배열을 선언하고, 배낭 용량이 K일 때, 들어간 물건들의 최대 가치합이 이 배열 속에 들어가게 된다. 또한, 1차원 dp 배열을 사용할 때에는 반드시 가방 용량을 **K부터 1까지 큰 쪽에서 작은 쪽의 순서로 살펴야 한다.**
$$
dp[j] = max(dp[j], dp[j-w[i]] + v[i])
$$
점화식은 이렇게 한 줄이다.

```javascript
for (let i = 1; i <= N; i++) {
    for (let j = K; j >= 1; j--) {
        if (j - w[i] >= 0) {
            dp[j] = Math.max(dp[j], dp[j - w[i]] + v[i])
        }
    }
}
```

과정을 일일이 쓸 수도 있겠지만, 다음에 언젠가 또 냅색 문제를 까먹어서 이곳에 다시 왔을 때 다시 그려보면서 이해하는 것이 좋을 것 같아 구체적인 과정을 그리지는 않는다.

일차원 배열을 활용해서 문제를 풀면 이차원을 사용했을 때보다 절반 정도의 시간만을 활용하게 된다. 