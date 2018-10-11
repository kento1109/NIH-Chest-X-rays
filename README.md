# NIH-Chest-X-rays

データセットの詳細は下記  
[NIH-Chest-X-rays](https://www.kaggle.com/nih-chest-xrays/data)

## 目的
胸部X線画像からEmphysema（肺気腫）を識別

## 訓練データ
画像についている疾患ラベルは、複数存在することもあるが、今回はラベルが1つ（Emphysema）のみの画像をターゲット群として抽出。  
コントロール群として、異常なし(No Findings)の画像をランダムに抽出。  

下図はターゲット群とコントロール群の画像をランダムに抽出したもの。

**Emphysema（肺気腫）**
![image](https://github.com/kento1109/NIH-Chest-X-rays/blob/master/Emphysema.png)

**No Findings（異常なし）**
![image](https://github.com/kento1109/NIH-Chest-X-rays/blob/master/No%20Finding.png)

尚、ラベルがEmphysemaのみの画像は892枚に対して、No Findingsの画像は60,361枚存在した。  
この場合、ラベル不均衡問題として別のアプローチが必要となるが、本実験では問題をシンプルにするため、No Findingsからランダムに1,000毎の画像を抽出してコントロール群とした。  
※詳細は[ノートブック](https://github.com/kento1109/NIH-Chest-X-rays/blob/master/data_analysis.ipynb)参照。

## 実験
実験は下記の3つモデルを用いて行った。
- 小さなCNNモデルを構築（LeNet）
- 転移学習+全結合層(Bottleneck features)
- 転移学習（Fine-tuning）

尚、今回の実験は限られたデータでの分類問題という点で下記のブログを参考とした。  
[Building powerful image classification models using very little data](https://blog.keras.io/building-powerful-image-classification-models-using-very-little-data.html)

また、転移学習元のモデルとしては下記の3つのモデルを用いて比較した。
- VGG16
- DenseNet
- InceptionV3

尚、対象としたサンプル1892枚(892+1000)のうち、10%を評価データに用いた。

## 結果
結果は下図の通り。実験は全て10エポックで実施した。  
※今回はモデル同士を比較するため、早期打ち切りは行っていない。  
![image](https://github.com/kento1109/NIH-Chest-X-rays/blob/master/summary.png)

## 評価
訓練データに関しては、DenseNet, InceptionV3の転移学習が高精度となった。特に、
- Bottleneck features(InceptionV3)
- Fine-tuning(DenseNet)
- Fine-tuning(InceptionV3)

は100%に近い精度で識別できていることが分かる。  
一方、VGG16での転移学習はうまく学習できなかった。

一方、評価データに関しては、学習が進んでも損失が小さくなることは確認できなかった。 精度に関しては、
- Bottleneck features(DenseNet)
- Bottleneck features(InceptionV3)

が75~80%近くと最も高精度の予測となった。  

## 考察
今回、訓練データで学習が上手く進んだモデルに関しても、明らかな過学習が見られた。  
これは、Emphysema（肺気腫）の画像のバラつきを吸収するだけの十分の画像が与えられなかったことが原因として考えられる。  
改善手法としてData Augmentationを試みたが、実験した限りでは精度の改善は見られなかった。  
（時間の制約上、Augmentationのパターンは限定的であったため、別のパターンで精度向上する可能性は考えられる。）
