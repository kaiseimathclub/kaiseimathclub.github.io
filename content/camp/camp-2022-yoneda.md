---
title: "「大きな数の計算」講義ノート"
date: 2022-08-30
lastmod: 2022-10-30
---

* **題名**：大きな数の計算
* **講師**：数学研究部 OB 米田 寛峻[^*]
* **日時**：2022 年 8 月 30 日
[^*]: 開成高等学校を 2021 年 3 月に卒業．現在は東京大学理科一類に所属．

# 1 はじめに

$179+443=622$, $243-234=9$, $28\times76=2128$, $2280\div24=95$, $\dots$ このような計算は，数学の問題を解くときだけでなく，日常生活でも行っていることでしょう．私たちが無意識に行っている数の計算には，どんな世界が広がっているのかを理解することが，本講義の目標です．

# 2 アルゴリズムと計算量

IT 化が進んだ現代では，「東京から熊谷まで行くのに何時間何分かかるか？」といったいろいろな問題がコンピューターによって解かれています．このような問題をプログラミングなどでどうやって解くのか，その具体的な手順のことを**アルゴリズム**といいます．アルゴリズムを考える上で重要となるのは，答えの計算にどのくらい時間がかかるかを表す「計算量」です．以下に，数学と関係が深い 3 つの例を挙げます．

## 2.1 $1+2+\cdots+N$ の計算

$1+2+\cdots+100$ の値を，単純に $1+2=3$, $3+3=6$, $6+4=10$, $\dots$, $4950+100=5050$ と計算すると，$99$ 回の足し算を必要とします．しかし，もっと効率的な方法がります．$1$, $2$, $\dots$, $100$ を「$1$ と $100$」「$2$ と $99$」…「$50$ と $51$」という $50$ 個のペアに分けてみましょう．どのペアの値の合計も $101$ になっているので，$50\times101=5050$ とすぐに答えが求まります．

$1+2+\cdots+N$ を計算するときも同じです．前者の方法は $N-1$ 回の足し算を必要としますが，後者の方法は $(N\div2)\times(N+1)$ という式で $3$ 回の四則演算しか必要としません．このように，問題の解き方によって効率に大きな差があります．

## 2.2 素数判定

自然数 $N$ が素数か判定する問題を考えます．素数とは $2$ 以上 $N-1$ 以下の整数で割り切れない数のことなので，それらの $N-2$ 個の数で割り切れないか確かめることで素数判定ができます．

しかし実は，合成数は必ず $\sqrt{N}$ 以下の約数を持つので，$2$ 以上 $\sqrt{N}$ 以下の整数で「試し割り」をするだけで十分です．もし全部で割り切れなければ，素数であることが分かります．

一般的なコンピューターでは 1 秒に $10^9$ 回程度の計算しか行えません．そのため，例えば $N=10^{16}$ 程度の数に対して素数判定をしたい場合，前者のアルゴリズムでは現実的な時間で計算が終わらないのに対し，後者のアルゴリズムでは一瞬で判定できます．

## 2.3 計算量の $O$ 記法

計算量の代表的な表し方として「$O$ 記法」があります．$O$ 記法は，計算回数が大まかにどの程度なのかを表します．素数判定の前者のアルゴリズムは計算回数が $N$ に比例するので計算量 $O(N)$ であり，後者のアルゴリズムは計算回数が $\sqrt{N}$ に比例するので計算量 $O(\sqrt{N})$ です．$O$ 記法では，計算回数の小さな項や定数倍は無視するのが一般的です．

# 3 四則演算の筆算

みなさんはおそらく小学校 3〜4 年で「筆算」を習ったと思います．これを思い出すと，筆算の計算量は以下のようになることが分かるでしょう．

{{<center>}}
| 四則演算 | 筆算の計算量 |
| :---: | :---: |
| $\text{($n$ 桁の整数)}+\text{($n$ 桁の整数)}$ | $O(n)$ |
| $\text{($n$ 桁の整数)}-\text{($n$ 桁の整数)}$ | $O(n)$ |
| $\text{($n$ 桁の整数)}\times\text{($n$ 桁の整数)}$ | $O(n^2)$ |
| $\text{($2n$ 桁の整数)}\div\text{($n$ 桁の整数)}$ | $O(n^2)$ |
{{</center>}}

足し算や引き算は筆算でも計算量 $O(n)$ であり，十分速く行えます．一方，掛け算や割り算は筆算だと計算量 $O(n^2)$ かかり，比較的時間がかかります．

# 4 高速な掛け算

$n$ 桁の掛け算は，筆算で行うと計算量 $O(n^2)$ かかりますが，実はもっと高速な方法があります．

## 4.1 カラツバ法

1960 年，当時 23 才だったソ連の数学者アナトリー・カラツバ[^1]は，$n$ 桁の掛け算を計算量 $O(n^{1.59})$ で行うアルゴリズムを発見しました．彼のアイデアは，$n$ 桁の掛け算を「$n/2$ 桁の掛け算 $3$ つ」を使って求める[^2]，というものです．

[^1]: Anatoly Alexeyevich Karatsuba (1937 - 2008)．
[^2]: 簡単のため，ここでは $n$ が偶数として考えますが，$n$ が奇数のときも真ん中あたりで分けると同様のことができます．

$n$ 桁の整数 $A=a_1\times10^{n/2}+a_0$（$a_1$, $a_0$ は $A$ の上 $n/2$ 桁と下 $n/2$ 桁）と $B=b_1\times10^{n/2}+b_0$（$b_1$, $b_0$ は $B$ の上 $n/2$ 桁と下 $n/2$ 桁）の掛け算を求めてみましょう．式の展開より，$$(a_1\times10^{n/2}+a_0)\times(b_1\times10^{n/2}+b_0)=a_1b_1\times10^n+(a_1b_0+a_0b_1)\times10^{n/2}+a_0b_0$$ であるから，$A\times B$ の計算には「$n/2$ 桁の掛け算」$4$ 回がかかります[^3]．ここで，$$a_1b_0+a_0b_1=(a_1+a_0)\times(b_1+b_0)-a_1b_1-a_0b_0$$ が成り立ちます．$a_1b_1$ と $a_0b_0$ は先ほどの式で既に計算されている値なので，追加で行うべき $n/2$ 桁の掛け算は $(a_1+a_0)\times(b_1+b_0)$ の $1$ 回だけです．まとめると，$n$ 桁の掛け算は

* $X=a_1\times b_1$
* $Y=a_0\times b_0$
* $Z=(a_1+a_0)\times(b_1+b_0)$

という $3$ つの「$n/2$ 桁の掛け算」を求める問題に落とし込むことができ，その答えが求まりさえすればあとは足し算・引き算だけで $$A\times B=X\times10^n+(Z-X-Y)\times10^{n/2}+Y$$ と計算できます．

[^3]: 「$\times10^n$」などは，数の右側に $0$ をいくつか書き加えるだけなので，計算量 $O(n^2)$ かかる掛け算には含めません．

## 4.2 カラツバ法の計算量

$n/2$ 桁の掛け算には計算量 $O((n/2)^2)$ かかるから，4.1 節の方法を使っても計算量を $3/4$ 倍にしかできていない，結局 $O(n^2)$ だ，と思うかもしれません．

ここで，$3$ つの「$n/2$ 桁の掛け算」にも，同じようにカラツバ法を適用してみましょう．そうすると，$9$ つの「$n/4$ 桁の掛け算」を求める問題に落とし込めます．同じように，

* $9$ 個の「$n/4$ 桁の掛け算」は，$27$ 個の「$n/8$ 桁の掛け算」に落とし込める
* $27$ 個の「$n/8$ 桁の掛け算」は，$81$ 個の「$n/16$ 桁の掛け算」に落とし込める
* （以下略）

と，より小さい掛け算の問題に分解していくことができます．「$\text{$1$ 桁}\times\text{$1$ 桁}$」のところまで分解すると，最初 $n=2^d$ 桁だった掛け算は，$1$ 桁の掛け算 $3^d$ 個になります．

このようにして，$2^d$ 桁の掛け算が計算量 $O(3^d)$ でできます．$n=2^d$ のとき $d=\log_2{n}$ なので，カラツバ法では $n$ 桁の掛け算にかかる計算量は $$O(3^d)=O(3^{\log_2{n}})=O(n^{\log_2{3}})=O(n^{1.5849\dots})$$ となり[^4]，筆算よりも高速である[^5]といえます．

[^4]: 対数 $\log_a{b}$ は，$a^x=b$ を満たす実数 $x$ のことを指します．これは高校 2 年で習う内容です．例えば $\log_3{243}=5$, $\log_2{3}=1.5849\dots$ などが具体例です．本講義では，特に断りがなければ，$\log{n}$ は $\log_2{n}$ を指すものとします．
[^5]: 実際は，桁数 $n$ が小さいときは筆算の方が速く，この理由は筆算の方が計算量にかかる「定数倍」が小さいからです．しかし，$n$ が一定以上になれば，カラツバ法の方が確実に速くなります．

## 4.3 高速フーリエ変換 (FFT) を使った掛け算

高速フーリエ変換 (FFT) や，これを $\bmod{p}$ 上で行った数論変換 (NTT) を使うと，$n$ 桁の掛け算を計算量 $O(n\log{n})$ で行うことができ，これが現在知られている催促のアルゴリズムです．FFT を説明するためには高校 3 年程度の数学の知識が必要なため，本講義では解説しないことにします．

これは，数百万桁程度の掛け算なら $1$ 秒もかからず行えることを意味します．次章以降では，特に断りなく $n$ 桁の掛け算を計算量 $O(n\log{n})$ で行えるものとします．

# 5 高速な割り算

先ほど，$n$ 桁の掛け算が計算量 $O(n\log{n})$ でできることを説明しました．では，割り算は筆算の計算量 $O(n^2)$ より速くできるのでしょうか？

## 5.1 アルゴリズムの設計図

$2n$ 桁の整数 $A$，$n$ 桁の整数 $B$ に対して，$A\div B$ の商を求めるために，以下の手順を考えます．

1. $B$ の逆数 ($=1/B$) を小数第 $2n$ 位まで求める．これを $M\times10^{-2n}$ とする．
2. $(A\times M)\times10^{-2n}$ の整数部分を計算する．これが $A\div B$ の商とほとんど一致する[^6]．

4 章で述べたように掛け算は計算量 $O(n\log{n})$ でできるので，あとは $B$ の逆数を高速に求められるかという問題になります．

[^6]: この計算で得られる値の誤差は $\pm1$ 以内になるので，実際の商がどうなるかは $\pm1$ の範囲を掛け算でチェックすれば分かる．

## 5.2 ニュートン法

ニュートン法とは，関数 $f(x)$ に対して $f(x)=0$ の解 $x$ を高速に求める手法です．以下のように解の近似値 $a_0$, $a_1$, $a_2$, $\dots$ を求めていくことで精度を桁数にして倍々に上げていきます．

1. 初期値 $a_0$ を設定する（これは $f(x)=0$ の解にある程度近い値にしたい）．
2. $n=0$, $1$, $2$, $\dots$ の順に以下を行う．
    * $a_{n+1}$ を「グラフ $y=f(x)$ の点 $(a_n,f(a_n))$ を通る接線と直線 $y=0$ の交点を $x$ 座標」とする．
3. 十分な精度が得られたら，2. のループを打ち切る．

手順 2. を数式で表すと，以下のようになります[^7]．$$a_{n+1}=a_n-\dfrac{f(a_n)}{f ^ \prime(a_n)}$$

[^7]: $f ^ \prime(x)$ は関数 $f(x)$ の微分を表します．微分は高校 2 年で習う内容ですが，まだ知らない方は「グラフの点 $(x,f(x))$ における傾き」ととらえておくと良いでしょう．

![camp-2022-yoneda](../../images/camp-2022-yoneda.png)

## 5.3 $1/B$ の計算

$1/B$ を小数第 $2n$ 位までの精度で求めることを考えます．まず，$f(x)=1/x-B$ としてニュートン法を適用しましょう．グラフ $y=f(x)$ の点 $(a_n,f(a_n))$ における傾きは $-1/a_n^2$ なので，$$a_{n+1}=a_n-\dfrac{f(a_n)}{f ^ \prime(a_n)}=a_n-\dfrac{1/a_n-B}{-1/a_n^2}=a_n+(a_n-Ba_n^2)=2a_n-Ba_n^2$$ という「割り算を使わない式」を使って $1/B$ を精度良く求められます．

初期値 $a_0$ を十分な精度[^8]で設定すれば，$a_1$, $a_2$, $\dots$ と近似値を更新していくにつれ精度の桁数が倍々になっていきます．以下の表は，$B=13$ の場合で，初期値を $a_0=0.05$ に設定してニュートン法を行った場合の計算結果です．

[^8]: 誤差 $2$ 倍ぐらいでも十分です．

{{<center>}}
| $n$ | $a_n$ | 精度（桁数） |
| :---: | :--- | :---: |
| $0$ | $0.05$ | $0$ 桁 |
| $1$ | $0.0675$ | $0$ 桁 |
| $2$ | $0.07576875$ | $1$ 桁 |
| $3$ | $0.0769057548046\dots$ | $3$ 桁 |
| $4$ | $0.0769230730223\dots$ | $7$ 桁 |
| $5$ | $0.0769230769230\dots$ | $14$ 桁 |
{{</center>}}

したがって，$a_{\log_2{n}}$ くらいまで求めれば十分です．精度 $2n$ 桁で $a_0$, $a_1$, $a_2$, $\dots$ を求めていくと，求めるべき $1/B$ の値が計算量 $O(n\log^2{n})$ で計算できます[^9]．

[^9]: この式は $n(\log{n})^2$ と同じ意味ですが，計算量の文脈では「$n\log^2{n}$」と書くことが多いです．

## 5.4 計算量の改善

5.3 節では $m\approx\log_2{n}$ として $a_0$, $a_1$, $\dots$, $a_m$ をすべて精度 $2n$ 桁で求めていましたが，$a_0$ を精度 $1$ 桁，$a_1$ を精度 $2$ 桁，$a_2$ を精度 $4$ 桁，…で求めていっても必要なループ回数はほとんど変わりません．計算回数は $$1\log_2{1}+2\log_2{2}+4\log_2{4}+8\log_2{8}+\cdots+n\log_2{n}=O(n\log{n})$$ となり，逆数 $1/B$ の小数点以下 $2n$ 桁が計算量 $O(n\log{n})$ で求まることを意味します．したがって，$A\div B$ の割り算は計算量 $O(n\log{n})$ でできます．

# 6 $\sqrt{2}$ の計算

$f(x)=x^2-2$ としてニュートン法を適用させると，$\sqrt{2}$ の値を精度良く求めることができます．グラフ $y=f(x)$ の点 $(a_n,f(a_n))$ における傾きは $2a_n$ なので，ニュートン法の漸化式は $$a_{n+1}=a_n-\dfrac{a_n^2-2}{2a_n}$$ となります．5.3 節と同様，精度は桁数にして倍々に増えていくので，$a_{\log_2{n}}$ くらいまでしか求める必要はありません．5.4 節で使ったテクニックと組み合わせると，$\sqrt{2}$ の小数点以下 $n$ 桁を計算量 $O(n\log{n})$ で求められます．

# 7 発展的なトピック

講義の残り時間によっては，発展的なトピックとして，例えば以下のものを扱う可能性があります．

* $n!$ の計算（計算量 $O(n\log^3{n})$）
* $e=2.71828\dots$ の計算（計算量 $O(n\log^2{n})$）
* $\pi=3.14159\dots$ の計算（計算量 $O(n\log^3{n})$）
* 素数判定（桁数を $n$ として，計算量 $O(n^2\log{n})$）

# 8 練習問題

本講義の内容を理解するために，いくつかの練習問題を挙げます．

{{<alert class="success">}}
**問題 1**

$20220827\times12340625$ をカラツバ法を使って計算しなさい．
{{</alert>}}

{{<alert class="success">}}
**問題 2**

$\sqrt[3]{2}$ の小数点以下 $n$ 桁を求める方法を考えなさい．
{{</alert>}}

{{<alert class="success">}}
**問題 3**

$f(x)=1/x^2-2$ として，初期値 $a_0=0.5$ でニュートン法を適用することを考える．
1. $a_{n+1}$ を，$a_n$ を使ったできるだけ簡単な式で表しなさい．
2. ニュートン法で最終的に求まる値を答えなさい．
3. 2\. の値を $2$ 倍すると，何が得られるか．
4. 3\. の結果が意味することについて論ぜよ．
{{</alert>}}

{{<alert class="success">}}
**問題 4**

カラツバ法では，$n$ 桁の掛け算を「$n/2$ 桁の掛け算」$3$ つに落とし込むことで，計算量 $O(n^{1.59})$ の掛け算を実現した．これと同じように，$n$ 桁の掛け算を「$n/3$ 桁の掛け算」$5$ つに落とし込むことで，掛け算を高速化することを考えたい．なお，この問題では，$A=a_2\times10^{2n/3}+a_1\times10^{n/3}+a_0$, $B=b_2\times10^{2n/3}+b_1\times10^{n/3}+b_0$ として考える．
1. もし上の目標が実現されれば，掛け算の計算量はどのくらいになるか．
2. $f(x)=(a_2x^2+a_1x+a_0)(b_2x^2+b_ax+b_0)$ として，$f(-2)$, $f(-1)$, $f(0)$, $f(1)$, $f(2)$ の値を求めよ．
3. $f(x)$ は $c_4x^4+c_3x^3+c_2x^2+c_1x+c_0$ という式で表される．$c_4$, $c_3$, $c_2$, $c_1$, $c_0$ を，2. で求めた 5 つの値を用いて表せ．
4. 3\. の結果が意味することについて論ぜよ．
{{</alert>}}

# 9 おわりに

本講義は，私が高校 3 年の時は執筆した記事「超高速！多倍長整数の計算手法」に基づいたものになっています．累計 7 万回以上閲覧され，感激しています．ぜひ読んでいただけるとうれしいです．

* 超高速！多倍長整数の計算手法【前編：大きな数の四則計算を圧倒的な速度で！】[https://qiita.com/square1001/items/1aa12e04934b6e749962]()
* 超高速！多倍長整数の計算手法【後編：N! の計算から円周率 100 万桁の挑戦まで】[https://qiita.com/square1001/items/def73e29dd46b156c248]()

最後に，この講義によって，数学だけでなくアルゴリズムや情報科学にも興味を持っていただけたならば，私としては本当はうれしい限りです．