# Memo

recall@10 が極めて高い。
10K train, 20K valid, 8d, normalized, euclid の条件で
recall@10 が 0.997(T), 0.975(V) になってる。
また non-normalized では 0.996(T), 0.969(V) 大筋で違いはない。

FP8とHNSWの合わせ技によるものか?
[faissでの実験](https://github.com/koron/jsts-vector-eval/blob/main/pycmd/recall.py)では32バイトを4バイトに圧縮していたが、
こちらはは8バイトで余裕がある。
faissを用いて同条件の8バイト、HNSWで実験すべきかもしれない。

以下はfaiss 8バイトPQでの実験結果。
実験に使ったスクリプトは [recall-8bytes.py](pycmd/recall-8bytes.py)。
非常に好成績になっており validation vectors に対する recall@10 は
voyager を凌駕してる。

```
Q-type  d       M       nbits   norm    T-num   V-num   k       recall  recall0
PQ      8       8       8       False   10000   20000   10      0.98823 0.98874
PQ      8       8       8       True    10000   20000   10      0.98511 0.98686
```

HNSWを加味したテストはまだできていない。
