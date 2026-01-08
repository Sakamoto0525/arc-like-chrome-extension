# today-tabs Specification

## Purpose
TBD - created by archiving change add-initial-specs. Update Purpose after archive.
## Requirements
### Requirement: Today Tabs の定義
Today Tabs は未整理の作業中 URL 一覧である。bucket="today" の Item がここに表示されるものとする（SHALL）。Today Tabs は自動で消さず、ユーザーが明示的に削除するまで保持されるものとする（SHALL）。

#### Scenario: Today Tabs の識別
- **GIVEN** Item が存在する
- **WHEN** Item の bucket が "today" である
- **THEN** その Item は Today Tabs に表示される

### Requirement: Today Tabs の表示
Today Tabs は Side Panel の Folder Tree の上に、未整理リストとして表示されるものとする（SHALL）。各 Item には×ボタン（削除）が表示される。

#### Scenario: Today Tabs 表示
- **GIVEN** Today Tabs に Item が存在する
- **WHEN** Side Panel が表示される
- **THEN** Today Tabs セクションに Item リストが表示される
- **AND** 各 Item に×ボタンが表示される

### Requirement: Today Tabs への自動追加（拡張経由）
拡張 UI（New Tab ボタン、Pins、Folder 内 Item、検索結果）から URL を開いた場合、Today Tabs の先頭に自動追加するものとする（SHALL）。

#### Scenario: 拡張 UI から URL を開く
- **GIVEN** ユーザーが Side Panel を開いている
- **WHEN** New Tab ボタン、Pin、Folder Item、検索結果のいずれかをクリックして URL を開く
- **THEN** その URL が Today Tabs の先頭に追加される（重複時は既存を先頭に移動）

### Requirement: Today Tabs への自動追加の対象外
Chrome 本体の「＋」新規タブ、アドレスバー入力、外部アプリ起点で開いたタブは Today Tabs に自動追加しないものとする（SHALL NOT）。

#### Scenario: Chrome 本体からタブを開く
- **GIVEN** ユーザーが Chrome 本体で新規タブを開く
- **WHEN** アドレスバーに URL を入力して移動する
- **THEN** Today Tabs には追加されない

### Requirement: Today Tabs の手動削除
ユーザーは Today Tab の×ボタンをクリックして、その Item を削除できるものとする（SHALL）。

#### Scenario: Today Tab 削除
- **GIVEN** Today Tabs に Item が存在する
- **WHEN** ユーザーが×ボタンをクリックする
- **THEN** その Item がストレージから削除される

### Requirement: Today Tabs は自動削除しない
システムは Today Tabs を自動的に削除・アーカイブしないものとする（SHALL NOT）。

#### Scenario: Today Tabs の自動削除禁止
- **GIVEN** Today Tabs に Item が存在する
- **WHEN** 時間が経過する（1日後、1週間後など）
- **THEN** Item は自動で削除されない

### Requirement: Today Tab から Folder への保存
ユーザーは Today Tab の Item を Folder に保存して Saved Item に変換できるものとする（SHALL）。MVP では、メニュー操作（コンテキストメニュー / ケバブメニュー等）による明示的な UI 操作で行うものとする（SHALL）。

#### Scenario: Today Tab を Folder に保存（メニュー操作）
- **GIVEN** Today Tabs に Item が存在する
- **WHEN** ユーザーがメニューから「Folder に保存」を選択し、保存先 Folder を指定する
- **THEN** Item の bucket が "saved" に変更される
- **AND** folderId が指定 Folder の ID に設定される
- **AND** Today Tabs から消える（bucket が "today" でなくなるため）

> **NOTE (MVP Scope)**: ドラッグ＆ドロップによる Today Tab → Folder 保存は MVP では実装しない。メニュー操作のみをサポートする。これは item-management spec の方針と整合している。

