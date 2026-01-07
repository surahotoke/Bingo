# ビンゴ
HTMLとCSSで作ったビンゴです。

## 実際に遊べるページ
https://surahotoke.github.io/Bingo

## プレイ動画


## 遊び方
1. はじめにビンゴ領域のどこかをクリックすると、毎回ランダムに配置されます。  
（この際、1~25のうちどれか1つの数字がFREEに置き換わります）
2. その後、好きなマスを押せるようになります。
3. ビンゴすると、その結果（ビンゴ / ダブルビンゴ / トリプルビンゴ）が表示されて操作できなくなり、ゲーム終了となります。

※ 空けるセル番号の表示は本実装とは異なる仕組みで行われるものとします。
## 対応ブラウザ・注意点
- 最新のChromeブラウザを推奨しています。Chrome / Edge / Operaで動作することを確認しました。（Firefox, Safariでは動作しませんでした）
- 稀に処理落ちにより、`エラー コード: 5`が表示されることがあるかもしれません。再読み込みで解消します。

## 構成ファイル
- `index.html`（25行）
- `style.css`（100行）
- `script.css`（185行 / うちアットルール90行）

## 仕組み
```mermaid
sequenceDiagram
    title 盤面ランダム化
    actor 人
    participant board_indeterminate_before as .board:has(>:indeterminate)::before
    participant board as .board
    participant board_checked as .board:has(>:checked)
    participant board_checked_label as .board:has(>:checked)>label
    board->>board: ランダムなseedにする
    人->>board_indeterminate_before: クリック
    board_indeterminate_before-->>board: こっちに吸われる
    board->>board_checked: 発動
    board->>board_checked: seed確定
    board_checked->>board_checked_label: seedを適用させてマスをバラバラにする
```

```mermaid
sequenceDiagram
    title ビンゴロジック
    actor 人
    participant label as .board>label
    participant label_checked as .board>label:has(:checked)
    participant label_checked_after as .board>label:has(:checked)::after
    participant judge_after as .judge::after
    participant judge as .judge
    participant board_before as .board::before
    人->>label: クリック
    label->>label_checked: 発動
    label_checked->>label_checked: ビンゴ判定を進める
    label_checked->>label_checked_after: 反映
    label_checked_after->>label_checked_after: このマスのビンゴ数デコード(0,1,2,3)
    label_checked_after->>label_checked_after: 全体ビンゴ数に加算
    label_checked_after->>judge_after: counter(bingo)
    judge_after->>judge_after: contentに設定
    judge_after->>judge: サイズ引き継ぎ
    judge->>judge: サイズ数値化
    judge->>board_before: 全体ビンゴ数継承
    board_before->>board_before: ビンゴしていればリザルト画面にする
```

## 自作乱数の精度の検証
Pythonを使って自作乱数の精度の検証を行いました。（こちらはAIに任せた）  
https://github.com/surahotoke/css-randomness-test

## 更新履歴
- 2025/01/07：初コミット