# Product Spec（MVP）

## 目的
Arc の「Spaces / Folders / Pins / Today Tabs」的な体験を、Google Chrome の拡張（Manifest V3）で再現する。
Arc の Split View / Peek は不要。実用性重視。

## コア概念
- Space：作業コンテキスト（Work / Private など）
- Folder：Space 内の階層構造（ディレクトリ）
- Item（Saved Item / 論理タブ）：保存された URL（起点URL）
- Pins：Space ごとのお気に入り（アイコン列）
- Today Tabs：未整理の作業中URL一覧（自動では消さない）

## UI（Side Panel）
上から順に以下のセクションを表示する（Arc-like）：
1. Header：Space Switcher / Settings
2. Search：常時表示の検索入力
3. Pins：横並びアイコン列（favicon or 犬アイコン）
4. New Tab：拡張経由の新規タブ作成ボタン
5. Today Tabs：未整理リスト（削除は×ボタン）
6. Folder Tree：フォルダ階層＋保存Item

## 重要仕様（Design Decisions）
### 管理対象
- 拡張が管理・表示するのは「論理タブ（Saved Items）」のみ。
- Chrome の実タブバーを置き換えない。実タブを自動分類しない。

### Today Tabs への自動追加
- 拡張 UI（New Tab、Pins、Folder、検索結果等）から開いたURLは Today Tabs の先頭に自動追加。
- Chrome 本体の「＋」新規タブ、アドレスバー入力、外部アプリ起点で開いたタブは Today Tabs に自動追加しない。
  - 管理したいなら「拡張の入口」を使う方針。

### Today Tabs の寿命
- Today Tabs は自動で消さない。
- ユーザーが明示的に削除する。

### Saved Item のクリック挙動（迷子リセット）
- Folder 内の保存Itemは「起点URL」として固定する。
- 保存Itemをクリックすると、常に item.url（起点URL）を新規タブで開く。
- 実タブ内でどれだけ遷移していても関係なく、再クリックで起点に戻れる。

### favicon
- favicon を取得できない場合はデフォルトの犬アイコンを表示する。

## Non-Goals（MVPではやらない）
- Split View
- Arc Peek
- Chrome 実タブの完全同期/置換
- Today Tabs の自動アーカイブ
- 実タブから論理タブを推測して自動生成
