# PlantDX

![version](https://img.shields.io/badge/version-1.0.0-red.svg)
![stars](https://img.shields.io/github/stars/yushin-ito/plantdx?color=yellow)
![commit-activity](https://img.shields.io/github/commit-activity/t/yushin-ito/plantdx)
![license](https://img.shields.io/badge/license-MIT-green)

<br>

## 📝 Overview

ATLANA は、BDF 製造プラントのデジタルトランスフォーメーションを実現する**次世代型 BDF 製造プラント運転システム**です。  
バイオディーゼル燃料製造プラントにおける技術的課題を解決するとともに、専門知識を必要としないユーザーフレンドリーな運転システムを構築します。

<br>

## 🔍 Background

観光地として知られる鳥羽市では、多くのホテルや飲食店が立地しています。そのため、日常的に多くの植物性廃油が排出されており、再利用されることなく廃棄されているのが現状です。  
また、海に面している鳥羽市では、漁業が盛んであるため、養殖業で多くの牡蠣殻も同様に廃棄されていました。  
そこで、鳥羽商船では植物性油と牡蠣殻を有効活用できる**バイオディーゼル燃料製造プラント**が提案された。

バイオディーゼル燃料（BDF）は植物性油を原料とする再生可能エネルギーであり、ディーゼルエンジンの燃料としての活用も可能です。  
化石燃料への依存から脱却することができるエネルギーとして注目されています。

しかし、この BDF 製造プラントは 3 つの問題点を抱えていました。

**1. プラント運転の属人化**  
廃油とメタノールを攪拌する工程において、その攪拌状態を適切に評価する手法が確立されておらず、その後の工程に悪影響を及ぼすリスクというものです。  
つまり、ほとんどが**暗黙知**すなわち**直感**によって運転されているのです。

**2. プラント運転の最適化**  
運転するために必要になる温度などのパラメーターの最適な値が判明していないというものです。  
つまり、ほとんどが**手探り**で運転されているのです。

**3. プラント運転の高度化**  
運転するためには高度な専門知識が要求さていて複雑になってきているというものです。  
つまり、限られた人しかプラントを運転することができません。

これらの課題は、現在のプラント産業にも共通した課題であり、早急に解決する必要がありました。

<br>

## 🚀 Features

この機能は、撹拌中の画像を解析して撹拌状態を定量評価することで、  
**暗黙知を排除した運転**を実現し、「プラント運転の属人化」を解決します。

以下の動画は、実際の撹拌直後から分離していく様子を撮影したものです。

<video controls>
  <source src="https://github.com/user-attachments/assets/181f9da1-6986-403f-bcc0-67959ab651fb" type="video/mp4" />
</video>

画像は**3 つの領域**に分かれており、上から空気領域、メタノール領域、廃油領域となっています。  
撹拌直後は、メタノールが廃油に液泡として溶け込んでいるため、時間の経過とともにメタノール領域は減少し、廃油領域が増加します。

この特徴に注目し、**画像中のメタノール領域の面積**を指標として、撹拌状態を定量評価することにしました。

以下の画像は、領域成長法を用いてメタノール領域を抽出した例です。

<div align="center">
  <img src="https://github.com/user-attachments/assets/f7415c0d-9e12-4753-b027-059af3eb54a2" alt="annotation" width="300px" height="300px" />
</div>

しかし、領域成長法は計算量が多く、**リアルタイムでの評価**が重要な今回のような用途には適していません。

そこで、画像解析の手法として **U-Net** を使用してメタノール領域を抽出することにしました。U-Net はセグメンテーションに特化した CNN で、エンコーダとデコーダから構成されます。

以下は、U-Net によって抽出したメタノール領域の画像です。

<div align="center">
  <img src="https://github.com/user-attachments/assets/c3d515e2-4ab9-4f22-b5ea-de0c9ddc4f5f" alt="segmentation" width="300px" height="300px"/>
</div>

これにより、**高速かつ高精度**にメタノール領域の面積を算出することが可能になりました。

以下のグラフは、メタノールの面積と、その体積を推定したものです。

<div align="center">
  <img src="https://github.com/user-attachments/assets/7a60c415-7473-4a70-a5d2-515146caccbb" alt="area" width="360px"/>
  <img src="https://github.com/user-attachments/assets/e0e186e4-4ec0-48b1-b57a-9eae35745fed" alt="volume" width="360px"/>
</div>

グラフからもわかる通り、時間が経つにつれ、**分離は緩やかに進行**していることが確認できます。

ATLANA では、画像から得られる情報をもとに**攪拌率**を定義する関数を構築しました。

### 2. 強化学習を用いたチューニング

この機能は、**強化学習（Reinforcement Learning）**を活用してヒーターの温度やポンプの圧力などの運転パラメーターを最適化することで、  
**利益を最大化する運転**を実現し、「プラント運転の最適化」に貢献します。

パラメーターを報酬関数とし、強化学習のアルゴリズムを用いて最適な行動を学習する仕組みです。

> [!CAUTION]
> この機能は現在開発中であり、仕様は今後変更される可能性があります。

### 3. 専門知識が必要ない運転システム

この機能は、実際のプラントの運転者の意見を元に Web アプリケーションを開発することで  
**専門知識を必要としない運転**を実現し、「プラント運転の高度化」を解決します。

以下の画像は、実際のダッシュボードのスクリーンです。

**フロー画面**

センサーデータをリアルタイムで表示する画面です。

<img src="https://github.com/user-attachments/assets/207f2610-c8bf-4839-8c24-468f175d6471" />

**分析画面**

過去のセンサーデータを時系列で可視化する画面です。

<div align="center">
  <img src="https://github.com/user-attachments/assets/bf0cdc81-4c71-48a9-8728-3c6fad35f080" alt="analytics" />
</div>

**制御画面**

アクチュエータをリモートで制御する画面です。

<div align="center">
  <img src="https://github.com/user-attachments/assets/18025f52-8740-4315-8536-e586f2668afb" alt="control" />
</div>

**ログ画面**

アクチュエータの制御に関するログを記録して表示する画面です。

<div align="center">
  <img src="https://github.com/user-attachments/assets/f0572643-b0b7-4f8a-83b1-10a3d3810cbb" alt="log" />
</div>

<br>

## 🔧 Usage

[![Open in VS Code](https://img.shields.io/static/v1?logo=visualstudiocode&label=&message=Open%20in%20Visual%20Studio%20Code&labelColor=2c2c32&color=007acc&logoColor=007acc)](https://open.vscode.dev/yushin-ito/plantDX)

1. リポジトリをクローンする

   ```bash
   git clone https://github.com/yushin-ito/plantDX.git
   ```

2. リポジトリに移動する

   ```bash
   cd app
   ```

3. 依存関係をインストールする

   ```bash
   npm install
   ```

4. 開発サーバーを起動する
   ```bash
   npm run dev
   ```

<br>

## 🛠️ Technology

- **Next.js**
- **Shadcn UI**
- **Supabase**
- **Vercel**
- **React Flow**
- **Recharts**

<br>

## ⚡️ Structure

```
plantdx/
├── public/             # アセット
├── src/
│   ├── actions/        # サーバーアクション
│   ├── app/            # ページ
│   ├── components/     # コンポーネント
│   ├── constants/      # 定数
│   ├── functions/      # ユーティリティ
│   ├── hooks/          # カスタムフック
│   ├── schemas/        # スキーマ
│   ├── styles/         # スタイル
│   └── types/          # 型定義
└── supabase/           # Supabase
```

<br>

## 🤝 Contributer

<a href="https://github.com/yushin-ito"><img  src="https://avatars.githubusercontent.com/u/75526539?s=48&v=4" width="64px"></a>
<a href="https://github.com/walterairs"><img  src="https://avatars.githubusercontent.com/u/98316678?s=48&v=4" width="64px"></a>

<br>

## 📜 LICENSE

[MIT LICENSE](LICENSE)
