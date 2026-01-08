# 必読：proposal 実行前に読むこと（Cursor用）

このリポジトリは「Arc-like な体験を Chrome 拡張（MV3）で再現する」プロジェクトです。
proposal コマンドを実行する前に、必ず以下のドキュメントを読み、内容に沿って提案・生成してください。

## 必読ドキュメント
1. docs/10_PRODUCT_SPEC.md
2. docs/20_ARCHITECTURE.md
3. docs/30_TECH_STACK.md

## 絶対に守ること（最重要）
- 管理対象は「論理タブ（Saved Items）」のみ。Chrome 実タブは置き換えない。
- Today Tabs は「拡張経由で開いたもののみ」自動追加。Chrome本体の + やアドレスバー入力は対象外。
- Today Tabs は自動で消さない（ユーザーが明示的に削除）。
- 保存Item（Folder内Item）は「起点URL」。クリックすると常に起点URLを新規タブで開く（迷子リセット）。
- favicon が取れない場合は犬アイコンを表示する。
- Split View / Peek は実装しない（Non-Goals）。

## proposal に期待するアウトプット
- リポジトリ構成案（MV3 + Side Panel + React）
- 主要ファイルの役割（manifest / background / sidepanel UI / storage）
- データモデル（Space/Folder/Item/Pin/TodayTabs）と永続化（chrome.storage.local）
- 最初のMVP実装順（storage → selectors → UI → CRUD）
