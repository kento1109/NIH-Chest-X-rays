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

## 実験
実験は下記の3つモデルを用いて行った。
- 小さなモデルを構築（LeNet）
- 転移学習+全結合層
- 転移学習（fine-tuning）

尚、今回の実験は限られたデータでの分類問題という点で下記のブログを参考とした。
[Building powerful image classification models using very little data](https://blog.keras.io/building-powerful-image-classification-models-using-very-little-data.html)


## 結果
![image](https://github.com/kento1109/NIH-Chest-X-rays/blob/master/summary.png)
