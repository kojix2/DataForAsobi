# テストデータ

## リファレンスゲノム

架空のリファレンスゲノムを生成した。

```python
import random

# 染色体名と長さを定義
chromosomes = {
    'chr1': 2000,
    'chr2': 1800,
    'chr3': 1600,
    'chr4': 1400,
    'chr5': 1200,
}

# 塩基をランダムに生成する関数
def generate_sequence(length):
    return ''.join(random.choice('ACGT') for _ in range(length))

# FASTAファイルを出力
for chromosome, length in chromosomes.items():
    sequence = generate_sequence(length)
    print(f">{chromosome}\n{sequence}")
```

```
python generate_reference.py > ref.fa
```

bwa のインデックスを作る

```
bwa index ref.fa
```

## 突然変異とショートリードのシミュレーション

[wgsim](https://github.com/lh3/wgsim) を使います

```
wgsim -N 3000 -S 1 ref.fa s1_1.fa s1_2.fa > mutation_s1.txt
wgsim -N 3000 -S 2 ref.fa s2_1.fa s2_2.fa > mutation_s2.txt
wgsim -N 3000 -S 3 ref.fa s3_1.fa s3_2.fa > mutation_s3.txt
```

bgzipによる圧縮

```
bgzip s*.fa
```

## 正常のサンプル

|name|read|file|
|----|----|----|
|S1|read1|s1_1.fa.gz|
|S1|read2|s1_2.fa.gz|
|S2|read1|s2_1.fa.gz|
|S2|read2|s2_2.fa.gz|
|S3|read1|s3_1.fa.gz|
|S3|read2|s3_2.fa.gz|
