---
title: Miller_Rabin
author: shjzhqm
date: 2020-09-09 08:15:00 +0800
categories: [Blogging]
tags: [Math]
mathjax: true
---

## Miller_Rabin

### 用途

快速判断一个数是否为质数

### 原理

#### 费马小定理

若 $p$ 为质数，则 $\forall a \in [1, p - 1]$ 有 $a ^ {p - 1} \equiv 1 (\mod p)$。

如果 $\forall a \in [1, p - 1]$ 有 $a ^ {p - 1} \equiv 1 (\mod p)$ 是否可以判断出 $p$ 为质数呢？很遗憾是不正确的，但是若 $\exists a \in [1, p - 1]$ 有 $a ^ {p - 1} \not \equiv 1 (\mod p)$ ，则 $p$ 一定是合数，因此我们可以选择其中几个 $a$ 来大致观察 $p$ 是否为质数。

#### 二次探测

如果 $p$ 是奇质数，则方程 $x ^ 2 \equiv 1 (\mod p)$ 仅有两根 $x = \pm 1$ ，因此若 $x \not = \pm 1$ 且 $x ^ 2 \equiv 1 (\mod p)$ 则 $p$ 一定是合数。

### 算法

令 $2 ^ s t = p - 1$ ，则每个数可以从 $d ^ t$ 开始验证，每次失败即可证明 $p$ 为合数，若成功将 $d ^ t$ 平方继续验证，平方次数不超过 $s$ ，如果所有数都满足则可认为 $p$ 为质数。

### 代码

```cpp
int n, k, prime[12] = {2, 3, 5, 7, 11, 13, 17, 19, 23, 29};

ll qpow(ll a, ll b, ll mod){
	__int128 ans = 1, base = a;
	while(b){
		if(b & 1) ans = ans * base % mod;
		base = base * base % mod;
		b >>= 1;
	}
	return ans;
}

bool Miller_Rabin(ll x){
	if(x == 2) return 1;
	if(x % 2 == 0 || x <= 1) return 0;
	for(int i = 0;i < 6;i++){
		if(x == prime[i]) return 1;
		if(qpow(prime[i], x - 1, x) != 1) return 0;
		ll d = x - 1, now = qpow(prime[i], d >> 1, x);
		while(!(d & 1)){
			if(now == x - 1) break; 
			if(now ^ 1) return 0;
			d >>= 1;
			now = sqrt(now);
		}
	}
	return 1;
}
```