# BUZZ KICK!!

夜の屋上で遊ぶ、1試合60秒の3Dフットサル。プレーに合わせて配信風のコメント・いいね・同接が反応する。

- `index.html` が本体（three.js r128 を cdnjs、GLTFLoader を jsDelivr から読み込み）
- `assets/skin-pro.json` はガチャの選手スキン用モデル（`assets/src/skin-pro.glb` を base64 にしたもの）。ローカルで開くときは `file://` ではなく簡易サーバー（例: `python3 -m http.server`）経由で開くとスキンも読み込める
- ブラウザで開くだけで遊べる。GitHub Pages にもそのまま置ける

## 操作
- PC: WASD/矢印で移動、J パス、K シュート（長押しでパワー、金色ゾーンで離すとPERFECT）、L ダッシュ・フェイント
- スマホ: 左半分がスティック、右にボタン

## 主な要素
BUZZレベルで能力アップ / MAXでBUZZルーレットと観客・警察の乱入 / コンボ / PERFECTシュート / 切り抜きリプレイ / XP・レベル・ガチャ（保存はブラウザ内）

### ガチャの中身（141種）
選手スキン・ボール・シュートの軌跡・ゴール演出に加えて、ステージ（空・コート・天気が変わる）、BGM（6曲）、看板、称号。称号の一部は実績でしかもらえない

### やり込み
- デイリーミッション（毎日3つ、全部達成でボーナス）とログインボーナス（7日周期）
- 実績96個（ガチャ券・かけら・実績限定の称号）と通算記録
- チャンネル登録者：試合ごとに増え、100人・1,000人・1万人・銀の盾・金の盾・ダイヤの盾の節目がある。登録者が多いほど配信開始時の同接も多い
- かけら：ガチャのかぶりでもらえ、交換所で好きなアイテムと交換できる
- 5レベルごとのレベル報酬、連勝中のXP倍率（最大1.5倍）

### ボールの物理
フットサル4号球（周囲62cm・420g・ローバウンド）を基準にした実物どおりの重力、空気抵抗、回転によるマグヌス効果（カーブ・バックスピンで浮く）、回転と摩擦を考えたバウンド（2mから落とすと59cm跳ね返る＝FIFAの規定 50〜65cm 内）、転がり抵抗。ポスト・ボード・フェンスのネット・ゴールネットはそれぞれ反発と摩擦が違う。CPUは同じ計算でボールの行き先を読み、シュートやループもこの軌道で狙う

### ご褒美の演出
ガチャマシンからカプセルが出て色が昇格していく演出（青→金→虹）、金確定・虹確定のカットイン、タップで開封、アイテムのプレビューつき結果カード、10連ガチャ（SR以上1枠確定）、SKIP、試合後のボーナススロット（777で大当たり）、いいねのCRITICAL（7%で3倍）、レベルアップや登録者の節目のお祝い画面、NEWバッジ

## BGM を音声ファイルに差し替える
BGM 6曲は今はプログラムで鳴らすシンセ。`index.html` の `BGM_FILES` に登録したBGMは、そのファイルを流す（ファイルが無い・読めないときはシンセのまま）。

```js
const BGM_FILES = { m_def:{ src:'assets/bgm/m_def.m4a', loop:[8.0, 96.0], gain:1 } };
```

- `loop`: 繰り返す区間（秒）。省略すると曲全体をループ
- 音量はシンセの音量に自動で揃える。`gain` で微調整
- 形式は m4a（AAC）か mp3。60〜120秒、128kbps 程度
- バズ中は再生速度が少し上がる

Suno で作る場合のプロンプト案（すべて Instrumental、歌なし）:

| id | 名前 | Style of Music |
|---|---|---|
| m_def | シティポップ | Japanese city pop, 100 BPM, funky slap bass, Rhodes electric piano, bright synth brass, disco drums, neon night rooftop, upbeat, loopable |
| m_lofi | ローファイ | lo-fi hip hop, 80 BPM, dusty boom bap drums, vinyl crackle, mellow jazzy piano chords, warm bass, chill night, loopable |
| m_chip | ピコピコ8bit | chiptune, 8-bit retro game, 132 BPM, square wave lead, fast arpeggios, energetic, loopable |
| m_trap | トラップ | trap beat, 140 BPM, heavy 808 bass, rolling hi-hats, dark synth pad, hype sports energy, loopable |
| m_euro | ユーロビート | eurobeat, 155 BPM, supersaw lead, fast synth arpeggios, driving bass, high energy, loopable |
| m_matsuri | お祭りビート | Japanese festival beat, 118 BPM, taiko drums, shinobue flute, shamisen, modern electronic drums, energetic, loopable |

Suno の無料プランで作った曲は商用利用できないので、公開・販売するなら有料プランで作った曲を使う。

## クレジット
- スキンのモーション: Mixamo の X Bot の骨格とキックモーション（`assets/motion-xbot.json`、メッシュは除去）。静止モデルのスキンは読み込み時にこの骨格へ自動でリギングし、キックはこのモーション、走りは同じ骨格の脚をプログラムで動かしている
- サッカーモーション: Mixamo「Soccer Game Pack」の54モーションをゲームの X Bot 骨格向けに変換したもの（`assets/motions-soccer.json`）。3Dスキン・3Dキーパーの走り（8方向）、パス、ヘディング、トラップ、スライディング、転倒と起き上がり、オーバーヘッド、キーパーのセーブ・キャッチ・スローなどに使う。実写スキンには読み込み時にその骨格へリターゲットして使う。素材単体での再配布はできないためこのリポジトリには含めず、無い場合は従来のプログラム制御のポーズで動く
- 選手スキンのモデル: Innerscene Corp（innerscene.com）作、CC0 1.0（パブリックドメイン）。ゲーム用にポリゴン数とテクスチャを軽量化し、付属のボールを除いて使用
