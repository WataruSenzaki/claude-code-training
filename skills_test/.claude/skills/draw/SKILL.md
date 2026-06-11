---
name: draw
description: 業務.md を読んで、判断と手間が主役の図を1枚のHTMLにし、ブラウザで開くスキル。流れは淡い背骨、重い手順と暗黙の判断を強調、判断対応表を主役に置く。図は文章の写し（正本は 業務.md）。フローチャートにはしない。非エンジニア向け（受講者ラベル「可視化」）。
---

# draw — 業務を可視化する（ラベル: 可視化）

`業務.md`（文章＝正本）を読み、**外部 JS なしの自己完結 HTML 1枚**にしてブラウザで開くスキル。
図は正本の**写し（view）**。中身を足したり解釈したりしない。直すときは図でなく文章（.md）を直し、もう一度このスキルで図にする。

## この図の約束（自分では言わない・設計の核）
- **フローチャートにしない**。流れ（背骨）は淡い灰色で軽く通すだけ。主役は**判断対応表**と**重い手順**。
- **足さない・解さない**。`業務.md` に無いことは図にしない（view だから）。
- **色で「重い理由」を繋ぐ**: 重い手順＝温色で太く／判断対応表の一番暗黙の行＝同じ温色を敷く。見た人が「この手順が重い＝この判断が頭の中だから」と一目で分かる。
- **楽にする入口**＝緑のカードで最下部（ここが直す出発点）。
- `## 手順の中身（手間・属人）` の表は**図に別表として出さない**。重い手順の強調＋楽にする入口で受ける（手間・属人の細部は正本の .md に残す）。図は際立たせる道具で、全部を写す台帳ではない。

## はじめに（毎回・自分では言わない準備）
- 同じフォルダの `業務.md` を読む。無ければ作らず止めて伝える:「先に業務をまとめましょう（/work）。まとまったら可視化できます。」
- 図は毎回 `業務.html` に上書きでよい（正本は .md なので失われない）。

## 手順

### 1. 業務まとめから部品を取り出す
`業務.md` から読み取る:
- **タイトル**（`# 業務: ◯◯`）
- **流れ（足場）**＝番号付きの手順列。各手順を短い箱の言葉にする（本人の言葉を変えない・縮めるだけ）
- **重い手順**＝`## 手順の中身` の表に出てくる手順名。流れの中で対応する箱を強調（heavy）にする。**複数該当すれば複数強調**（例: 残数の確認が依頼時・前日の2回出るなら両方）
- **判断対応表**＝全行。先頭行（または「一番暗黙」と書かれた行）を強調行（implicit）にする
- **楽にする入口**＝1〜2点

### 2. 下のひな形に流し込む（見た目は固定・中身だけ差し替え）
`<style>` はそのまま使う。`<body>` の `<!-- 差し替え -->` の中身だけ業務まとめの部品に置き換える。下は実例（原料残数チェック）入りの完成形。これを土台に中身を入れ替える。

```html
<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>業務</title>
<style>
:root{
  --ink:#1f2937; --muted:#9ca3af;
  --faint-bg:#eef1f4; --faint-line:#cbd5e1;
  --heavy:#ea7a3b; --heavy-bg:#fdf0e6; --heavy-line:#ea7a3b;
  --panel:#ffffff; --line:#e5e7eb;
}
*{box-sizing:border-box;}
body{margin:0; padding:40px 24px; font-family:"Meiryo","Hiragino Kaku Gothic ProN","Yu Gothic",sans-serif; color:var(--ink); background:#f7f8fa;}
.sheet{max-width:960px; margin:0 auto;}
h1{font-size:22px; margin:0 0 4px;}
.sub{color:var(--muted); font-size:13px; margin-bottom:28px;}
.section-label{font-size:13px; color:var(--muted); letter-spacing:.04em; margin:0 0 10px;}
.flow{display:flex; align-items:stretch; flex-wrap:wrap; gap:0; margin-bottom:6px;}
.step{flex:1 1 0; min-width:110px; background:var(--faint-bg); color:#6b7280; border:1px solid var(--faint-line); border-radius:10px; padding:12px 10px; font-size:13px; line-height:1.4; text-align:center; display:flex; flex-direction:column; align-items:center; justify-content:center;}
.arrow{align-self:center; color:var(--faint-line); padding:0 6px; font-size:16px;}
.step.heavy{background:var(--heavy-bg); color:#9a4a16; border:2px solid var(--heavy-line); font-weight:700;}
.step .tag{display:block; font-size:11px; font-weight:700; color:var(--heavy); margin-top:4px;}
.bridge{text-align:center; margin:14px 0 18px; color:var(--heavy); font-size:13px;}
.bridge .down{font-size:20px; line-height:1;}
.panel{background:var(--panel); border:1px solid var(--line); border-radius:12px; padding:18px 20px; margin-bottom:22px; box-shadow:0 1px 2px rgba(0,0,0,.03);}
.panel h2{font-size:16px; margin:0 0 12px; display:flex; align-items:center; gap:8px;}
.panel h2 .star{color:var(--heavy);}
table{width:100%; border-collapse:collapse; font-size:13.5px;}
th{text-align:left; color:#6b7280; font-weight:600; font-size:12px; padding:8px 10px; border-bottom:2px solid var(--line);}
td{padding:11px 10px; border-bottom:1px solid var(--line); vertical-align:top; line-height:1.5;}
tr.implicit td{background:var(--heavy-bg);}
tr.implicit td:first-child{font-weight:700;}
.mark{display:inline-block; font-size:11px; color:var(--heavy); font-weight:700; margin-left:6px; white-space:nowrap;}
.who{color:#6b7280; white-space:nowrap;}
.exit{background:#f0f7f2; border:1px solid #c9e3d2; border-left:5px solid #4a9d6e; border-radius:10px; padding:16px 20px;}
.exit h2{font-size:15px; margin:0 0 8px; color:#2f7a52;}
.exit ul{margin:0; padding-left:18px;}
.exit li{margin:6px 0; line-height:1.55; font-size:13.5px;}
.exit b{color:#1f6e46;}
</style>
</head>
<body>
<div class="sheet">
  <!-- 差し替え: タイトル -->
  <h1>業務: 翌日の原料残数チェック</h1>
  <div class="sub">流れは薄く・判断と手間が主役</div>

  <div class="section-label">流れ（足場）</div>
  <!-- 差し替え: 流れ。各手順を .step に。重い手順は class="step heavy" ＋ <span class="tag">重い手順</span>。箱の間に <div class="arrow">&rarr;</div> -->
  <div class="flow">
    <div class="step heavy">依頼を見て残数確認<span class="tag">重い手順</span></div>
    <div class="arrow">&rarr;</div>
    <div class="step">未反映を仮計上</div>
    <div class="arrow">&rarr;</div>
    <div class="step">発注依頼</div>
    <div class="arrow">&rarr;</div>
    <div class="step heavy">前日に再度 残数確認<span class="tag">重い手順</span></div>
    <div class="arrow">&rarr;</div>
    <div class="step">秤量・記載</div>
  </div>

  <div class="bridge">
    <div class="down">&#9660;</div>
    <!-- 差し替え可: 重い理由の一言（判断起点なら「判断が頭の中」、手間起点なら手間に触れる） -->
    この手順が重いのは、下の判断が「頭の中」にあるから
  </div>

  <div class="panel">
    <h2><span class="star">&#9733;</span>判断対応表 <span style="font-size:12px;color:#9ca3af;font-weight:400;">（この仕事の主役）</span></h2>
    <table>
      <thead><tr><th>こうしたい・こういう時</th><th>だからこうする</th><th>今この判断を持つ人</th></tr></thead>
      <!-- 差し替え: 判断対応表の全行。先頭(一番暗黙)の行に class="implicit" ＋ 1列目に <span class="mark">一番暗黙</span> -->
      <tbody>
        <tr class="implicit">
          <td>正確な前残数を出したい<span class="mark">一番暗黙</span></td>
          <td>帳簿で未反映の製造を特定し、製造結果データから消費を差し引く</td>
          <td class="who">自分（製造管理責任者）</td>
        </tr>
        <tr>
          <td>帳簿反映が事務さんの忙しさでバラバラ</td>
          <td>反映済みの範囲を見極め、範囲外の直近消費を自分で拾って仮計上</td>
          <td class="who">自分（記憶で覚えている）</td>
        </tr>
        <tr>
          <td>どのロットから使うか</td>
          <td>先入先出のルールで「何が先か」を決める</td>
          <td class="who">自分（定義が頭の中）</td>
        </tr>
        <tr>
          <td>何をどれだけ使うか</td>
          <td>仕様書どおりに決まる（自分の判断ではない）</td>
          <td class="who">仕様書（過去に確定）</td>
        </tr>
      </tbody>
    </table>
  </div>

  <div class="exit">
    <h2>&#128161; 楽にする入口（ここから直す）</h2>
    <!-- 差し替え: 楽にする入口 1〜2点。<b>判断起点</b> / <b>プロセス起点</b>（(d)で出た場合のみ）の見出し付きで -->
    <ul>
      <li><b>判断起点</b>：「帳簿の未反映ぶんを記憶で特定する」 &rarr; 表・リスト化で誰でも判断できる（属人が解ける）</li>
      <li><b>プロセス起点</b>：帳簿の反映が即時になれば、この確認作業ごと不要になる（(d)で出た場合のみ）</li>
    </ul>
  </div>
</div>
</body>
</html>
```

### 3. 書いて開く
- 今いるフォルダの `業務.html` に書く。
- ブラウザで開く: Windows は `start "" "<フルパス>\業務.html"`（PowerShell なら `Start-Process "<フルパス>\業務.html"`）／ Mac は `open "<パス>/業務.html"`。
- 伝える:「図ができて、ブラウザで開きました。流れは薄い線、**色がついた手順と表が、この仕事の重いところ**です。一番下が楽にする出発点。」

### 4. 図を見て確認する（L3 への入口・必ず行う）
図を開いた直後に、**1問ずつ順番に**聞く:

**(1) まず「合っているか」だけを聞く**（AskUserQuestion で）
- 「見た内容は合っていますか？」
- 合っていない → `業務.md`（文章＝正本）を口頭指示で直し、もう一度この draw スキルで図を作り直してから (1) に戻る。図を直接いじらない。
- 合っている → (2) へ進む。

**(2) 合っていることを確認してから「直したいところ」を聞く**（AskUserQuestion で）
- 「直したいところはありますか？」
- ある → `業務.md` を直して図を作り直す。
- ない → 次へ進む。

### 5. 終わったら次へ進む（連鎖・受講者に操作させない）
確認が取れたら、受講者に何も打たせず、**そのまま L3「AIに相談する」へ進む**。先にこう一言だけ伝える:
「では、この"楽にする入口"を使って、AI に相談してみましょう。最初に方向を選びます——今の運用のままで楽にしたいか、運用ごと変えて根本から解決したいか。」
