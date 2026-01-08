# item-management Specification

## Purpose
TBD - created by archiving change add-initial-specs. Update Purpose after archive.
## Requirements
### Requirement: Item の定義
Item（Saved Item / 論理タブ）は保存された URL（起点 URL）を表す。Item は Folder に属するか、または Today Tabs として未整理状態で存在するものとする（SHALL）。システムは Chrome の実タブを置き換えず、論理タブのみを管理する（SHALL）。

#### Scenario: Item の識別
- **GIVEN** システムに Item が存在する
- **WHEN** Item を参照する
- **THEN** 一意の ID で識別できる

#### Scenario: Item のプロパティ
- **GIVEN** Item が存在する
- **WHEN** Item の情報を取得する
- **THEN** URL（起点URL）、タイトル、favicon URL、bucket（"today" | "saved"）、所属 Folder ID（saved の場合）を含む

### Requirement: bucket と folderId の関係
bucket="saved" の Item は必ず folderId を持つものとする（SHALL）。bucket="today" の Item は folderId を持たない（null）ものとする（SHALL）。

#### Scenario: Saved Item の folderId
- **GIVEN** bucket="saved" の Item が存在する
- **WHEN** Item のプロパティを確認する
- **THEN** folderId は null ではなく、有効な Folder ID を持つ

#### Scenario: Today Tab の folderId
- **GIVEN** bucket="today" の Item が存在する
- **WHEN** Item のプロパティを確認する
- **THEN** folderId は null である

### Requirement: Item の作成（Folder に保存）
ユーザーは URL を Folder 内に Item として保存できるものとする（SHALL）。

#### Scenario: Item 保存
- **GIVEN** Folder が存在する
- **WHEN** ユーザーが URL を Folder に保存する
- **THEN** bucket="saved" の Item が作成され、指定 Folder に追加される

### Requirement: Item の削除
ユーザーは Item を削除できるものとする（SHALL）。Item を削除した場合、その Item に関連する全ての参照（Pins、Today Tabs 表示）も同時に削除されるものとする（SHALL）。

#### Scenario: Saved Item 削除
- **GIVEN** Folder 内に Item が存在する
- **WHEN** ユーザーが Item を削除する
- **THEN** Item がストレージから削除される

#### Scenario: Item 削除時の Pin 参照削除
- **GIVEN** Item が Pins に追加されている
- **WHEN** その Item を削除する
- **THEN** Item がストレージから削除される
- **AND** Pins リストからその Item ID が削除される

#### Scenario: Item 削除時の整合性
- **GIVEN** Item が存在する
- **WHEN** その Item を削除する
- **THEN** 全ての関連参照（pins.bySpaceId 内の itemId）が削除される
- **AND** 孤立した参照は残らない

### Requirement: Item の移動
ユーザーは Item を別の Folder に移動できるものとする（SHALL）。MVP では Item の移動はコンテキストメニュー等のメニュー操作で行うものとする（SHALL）。ドラッグ＆ドロップによる移動は MVP のスコープ外とする。

#### Scenario: Item 移動（メニュー操作）
- **GIVEN** Item が Folder A に存在する
- **WHEN** ユーザーがメニューから「移動」を選択し、Folder B を指定する
- **THEN** Item の所属 Folder が B に更新される

> **NOTE (MVP Scope)**: ドラッグ＆ドロップによる Item 移動は MVP では実装しない。メニュー操作（右クリック / ケバブメニュー等）による移動のみをサポートする。

### Requirement: Item の名前（タイトル）変更
ユーザーは Item のタイトルを変更できるものとする（SHALL）。

#### Scenario: Item 名前変更
- **GIVEN** Item が存在する
- **WHEN** ユーザーが新しいタイトルを入力して確定する
- **THEN** Item のタイトルが更新される

### Requirement: Today Tab から Saved Item への変換
ユーザーは Today Tab の Item を Folder に保存して Saved Item に変換できるものとする（SHALL）。MVP ではメニュー操作で行うものとする（SHALL）。

#### Scenario: Today Tab を Folder に保存（メニュー操作）
- **GIVEN** Today Tab の Item が存在する
- **WHEN** ユーザーがメニューから「Folder に保存」を選択し、保存先 Folder を指定する
- **THEN** Item の bucket が "saved" に変更され、指定 Folder に所属する
- **AND** folderId が指定 Folder の ID に設定される

> **NOTE (MVP Scope)**: ドラッグ＆ドロップによる Today Tab → Folder 保存は MVP では実装しない。

### Requirement: Saved Item のクリック挙動（迷子リセット）
Saved Item（Folder 内の Item）をクリックすると、常に起点 URL を新規タブで開くものとする（SHALL）。実タブ内でどれだけ遷移していても、再クリックで起点に戻れる。

#### Scenario: Saved Item クリック
- **GIVEN** Folder 内に Item（起点URL: https://example.com）が存在する
- **WHEN** ユーザーが Item をクリックする
- **THEN** 常に https://example.com が新規タブで開かれる

### Requirement: favicon の表示
Item の favicon を表示する。favicon が取得できない場合は、デフォルトの犬アイコンを表示するものとする（SHALL）。

#### Scenario: favicon 表示
- **GIVEN** Item の favicon URL が有効である
- **WHEN** Item が表示される
- **THEN** favicon が表示される

#### Scenario: favicon 取得失敗時
- **GIVEN** Item の favicon URL が無効または取得不可
- **WHEN** Item が表示される
- **THEN** デフォルトの犬アイコンが表示される

### Requirement: Today Tabs における URL の一意性
Today Tabs では同一 URL（文字列完全一致）の Item は1つのみ存在できるものとする（SHALL）。重複する URL を Today Tabs に追加しようとした場合、新規 Item を作成せず、既存 Item を再利用するものとする（SHALL）。

#### Scenario: Today Tabs への重複 URL 追加
- **GIVEN** Today Tabs に URL "https://example.com" の Item が存在する
- **WHEN** 同じ URL "https://example.com" を拡張 UI から開く
- **THEN** 新しい Item は作成されない
- **AND** 既存の Item が Today Tabs の先頭に移動する

#### Scenario: URL の完全一致判定
- **GIVEN** Today Tabs に URL "https://example.com/page" の Item が存在する
- **WHEN** URL "https://example.com/page?param=1" を開く
- **THEN** 文字列が完全一致しないため、新しい Item が作成される

> **NOTE**: URL の一意性判定は文字列完全一致で行う。正規化（末尾スラッシュ、クエリパラメータ順序等）は行わない。これは design-decisions の「重複 URL の処理」要件に準拠している。

