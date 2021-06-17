# BirdCLEF-2021

## 2021/5/4
* kkillerのbaselineのseedを変えたものを個人で提出
* アンサンブルで0.62のスコアが出た

## 2021/5/5
* pink noiseやGaussian noiseを加えてメルスペクトログラムを計算しなおした
* データ数が1/4のまま学習してしまいスコアは0.36や0.42と低かった

## 2021/5/6
* すべてGaussian noiseで学習 0.51

## 2021/5/7
* 全データにピンクノイズとボリュームコントロールを適用した画像で学習したモデルを提出 0.58
* 上のモデルとkkillerのモデルとkkilerのシードを変更したものでアンサンブル 0.62

## 2021/5/8
* Gaussian noise, always=False(確率0.5でノイズを付加)で学習 0.54
  * すべてGaussian noiseのものに比べて0.03上昇
* Gaussian noiseとpink noiseをalways=Falseで計算したもので学習　***0.63***
## 2021/5/9
* Gaussian noiseとpink noiseをalways=Falseで付加したデータを用いて、seedを変更して作った5つのモデル(seed=1316, 8823, 74298, 123456, 1999316)でアンサンブル　***0.64***
## 2021/5/11
* resnest50に異常アリ。事前学習をさせたresnest50をインターネットから取ってくる部分のエラー?  
これのせいで、学習部分のコードに事前学習をしていないresnestを使用する変更あり。
torchhubの不具合が戻れば治るはず？
## 2021/5/12
mixupのepoch合計55にしたやつ(Fork of submit_gnoise50per_vcontrol_seed7_epoch10)https://www.kaggle.com/udondon/fork-of-submit-gnoise50per-vcontrol-seed7-epoch10?scriptVersionId=62628971
提出0.59  
mixupのepoch合計100にしたやつ(submit_mixup_epoch100)https://www.kaggle.com/udondon/submit-mixup-epoch100?scriptVersionId=62667329
提出0.59
## 2021/5/13
そうへいが提出した（ensemble noises and kkillers part3）　　0.63
これは何と何のアンサンブル？

室真のましましのepochを10から30にしたもの（gnoise50_pinknoise50_epoch30） 0.58

## 2021/5/14
specaugmentエポック20_5widthを確率50%で付与する（submit_SpecAugment_epoch20_）　0.59
submit_SpecAugment_epoch20_ エポックを30にしたやつ

## 2021/5/15
MixUpのラベルをKaramarenkoが実装していた方法で計算した。
(primary_labelの鳥は1.0、secondary_labelsの鳥は0.3、それ以外の鳥は0.0)
* 閾値を0.8 f1=0.57
* 閾値を0.5 f1=0.39


閾値を0.5にしたときはrecallが大きくなりすぎているのだろう。
閾値をもう少し上げた場合も試してみたい。
出力をsigmoidで計算しているので、softmaxを計算して確率に変更してから
推論を行ってもよいかもしれない。

## 2021/5/16
SpecAugmentの改良を行った。  
今までは確率50%で5widthの隠しを入れていたが、今回は確率そのままで幅を１５にしたもの、
確率を１００％にして同じ幅で隠しをいれたもの。  
submit_SpecAugment_15width_50per_add  ０．６０  
SpecAugment 100%width5 Inference  　0.61

## 2021/5/17
resnest200、epochs=12で学習した。 
imagesはPink and Gaussian Noise(always = False)を加えた音声のmel   
score=0.56

SpecAugmentの15widthを確率90％で付加したもので学習し提出  
notebook("submit_SpecAugment_15width_90per_add") score0.60  
確率50％で付加した時とスコアの変化なし.  
## 2021/5/18  
"submit_SpecAugment_10width_90per_add"  
SpecAugmentの10widthを確率90％で付加したもので学習し提出 スコア0.58  
確率90％なら、widthをふやしていった方がスコアは高くなっているかもしれない  
理由は、SpecAugmentの15widthを確率90％で付加したもので学習し提出したnotebook("submit_SpecAugment_15width_90per_add")のscoreが0.60だから。  
あと、5widthの90％をして0.58よりスコアが低かったら、この仮説の根拠になる。  

## 2021/5/19
画像を逐一読み込んできてノイズを加えるようにして学習を行った。
ピンクノイズ、ガウスノイズ、SpecAugmentをそれぞれ90%の確率で行った。
* 12epoch [Noise Noise](https://www.kaggle.com/soule50431/pink-gauss-specaug-90) 0.57
* 12+12epoch [Pink Gauss Spec 90% 24epoch](https://www.kaggle.com/soule50431/pink-gauss-spec-90-24epoch) 0.55


## 2021/5/20
今までのモデルは全データで学習していないことに気づいた。
昨日と同じデータで0から24epoch全データで学習させた。
* [Pink Gauss Spec 90% 24epoch](https://www.kaggle.com/soule50431/pink-gauss-spec-90-24epoch) 0.59

0から24epoch学習させたのでスコアが下がらなかったとすれば、スコアが上がった要因は全データで学習させたことか？
もしそうなら今までの0.61付近のスコアが出ていたモデルを全データで学習し直してアンサンブルをすれば0.6後半が狙えそう？

* width5で#型にSpecAugmentをし、全データで12epoch学習させた。　

## 2021/5/21
* kkillerのノイズなしデータで12epoch [kkiller all data training](https://www.kaggle.com/soule50431/kkiller-all-data-inference?scriptVersionId=63435229)
* PinkNoise GaussianNoise 50% + VolumeControl 100%のデータを用いてresnest50で12epoch [PinkAndGaussFull_train](https://www.kaggle.com/murosin/pinkandgaussfull-train)

どちらも全データを用いるように変更したので学習が足りていない可能性がある。

## 2021/5/22
* kkillerのノイズなしデータで30epoch 0.57 [Fork of kkiller all data Inference](https://www.kaggle.com/soule50431/fork-of-kkiller-all-data-inference?scriptVersionId=63610681)
* MAX_READ_SAMPLE=3に変更し、epoch30　0.55 [ノート](https://www.kaggle.com/murosin/clean-fast-simple-bird-identifier-inference/data?scriptVersionId=63622801)
## 2021/5/23
* 丼型のspecaugment_２width 0.60　epoch12 [submit_SpecAugment_2width_twice_90per_add](https://www.kaggle.com/udondon/submit-specaugment-2width-twice-90per-add?scriptVersionId=63720559)
* 音声のクオリティを3.5以上のデータだけで学習(データが事前に画像を読み込まずに逐一読み込む)　
[kkiller all data limit audio quality Inference](https://www.kaggle.com/soule50431/kkiller-all-data-limit-audio-quality-inference?scriptVersionId=63714001) 0.57

## 2021/5/24
kkillerのモデルを用いて閾値を0.1に変更するだけで0.64のスコアを出している[ノート](https://www.kaggle.com/marbury/simple-bird-identifier-inference)があった。
なので、今までの自分たちの良いスコアを出していたモデル推論の閾値を変更してみた。

pink + gauss + volumecontrolのモデルの閾値を変更した。
* Threshold = 0.11 スコア 0.5 [ノート](https://www.kaggle.com/udondon/simple-bird-identifier-inference?scriptVersionId=63785447)
* Threshold = 0.52 スコア 0.61 [ノート](https://www.kaggle.com/murosin/clean-fast-simple-bird-identifier?scriptVersionId=63787721)


## 2021/5/25
* pink + gauss + volumecontrolの閾値を0.35に変更 [Simple Bird Identifier_theresh0.35 [Inference]](https://www.kaggle.com/udondon/simple-bird-identifier-theresh0-35-inference?scriptVersionId=63786671) 0.63
* SpecAugment 5width 100% の閾値を0.35に変更 [SpecAugment 100%width5 Inference thresh change](https://www.kaggle.com/murosin/specaugment-100-width5-inference-thresh-change?scriptVersionId=63789623) 0.62

## 2021/5/26
* submit_mixup_epoch100 thresh 0.2  0.60
* submit_mixup_epoch100 thresh 0.3  0.58  

## 2021/5/27
* submit_mixup_epoch100 thresh 0.15 スコア0.59  
* submit_mixup_epoch100 thresh 0.10 スコア0.55  

## 2021/5/28  
* Thresh 0.30 uniform distribution 0.56
* Thresh 0.985 Mixup Karamarenko Infer 0.58

## 2021/5/29  
むろしんのましましの閾値を変えたもので実験
* submit_ensemble_thresh0.30 | Version 1 0.62  
* submit_ensemble_thresh0.11 0.59

## 2021/5/30
specaugment THRESH = 0.35 pink and gauss noise THRESH = 0.25 kkillerのモデル THRESH = 0.11
* 以下の4つをアンサンブル、閾値は平均 スコア　0.65
  * kkiller
  * SpecAugment 100% width5(thresh=0.25)
  * Pink + Gauss + VolumeControl(thresh=0.35)
  * 丼SpecAugment 2width(thresh=0.25)
* 以下7つvoting型のアンサンブルしたもの 0.65
  * kkiller
  * SpecAugment 100% width5
  * Pink + Gauss + VolumeControl シード変更したもの5つ

## 2021/5/31  
* 以下の7つをvoting 2/7 0.64
  * Pink + Gauss + VolumeControl(thresh=0.53)
  * SpecAugment 100% width5(thresh=0.50)
  * MixUp Karamarenko(thresh=0.9)
  * kkiller(thresh=0.11)
  * SpecAugment 100% width5(thresh=0.45)
  * 俺らのMixUp(thresh=0.3)
  * pink + Gauss + SpecAugment(thresh=0.4)
* 以下7つ平均 0.65
  * kkiller(thresh=0.11)
  * SpecAugment 100% width5(thresh=0.25)
  * Pink + Gauss + VolumeControl シード変更したもの5つ(thresh=0.25)
