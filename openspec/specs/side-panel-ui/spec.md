# side-panel-ui Specification

## Purpose
TBD - created by archiving change add-initial-specs. Update Purpose after archive.
## Requirements
### Requirement: Side Panel の使用
UI は Chrome の Side Panel 上に React アプリとして構築するものとする（SHALL）。

#### Scenario: Side Panel 表示
- **GIVEN** 拡張がインストールされている
- **WHEN** ユーザーが拡張アイコンをクリックする
- **THEN** Side Panel が開き、React UI が表示される

### Requirement: UI セクション構成
Side Panel は上から順に以下のセクションを表示するものとする（SHALL）：
1. Header：Space Switcher / Settings
2. Search：常時表示の検索入力
3. Pins：横並びアイコン列（favicon or 犬アイコン）
4. New Tab：拡張経由の新規タブ作成ボタン
5. Today Tabs：未整理リスト（削除は×ボタン）
6. Folder Tree：フォルダ階層＋保存 Item

#### Scenario: セクション順序
- **GIVEN** Side Panel が開いている
- **WHEN** UI を確認する
- **THEN** Header → Search → Pins → New Tab → Today Tabs → Folder Tree の順で表示される

### Requirement: Header セクション
Header には Space Switcher（ドロップダウンまたはタブ）と Settings アクセスを配置するものとする（SHALL）。

#### Scenario: Header 表示
- **GIVEN** Side Panel が開いている
- **WHEN** Header を確認する
- **THEN** 現在の Space 名と切り替え UI が表示される

### Requirement: Pins セクション
Pins は favicon（または犬アイコン）の横並びアイコン列として表示するものとする（SHALL）。

#### Scenario: Pins アイコン表示
- **GIVEN** 現在の Space に Pins が設定されている
- **WHEN** Pins セクションを確認する
- **THEN** アイコンが横一列に並んで表示される

### Requirement: New Tab ボタン
New Tab ボタンは Pins と Today Tabs の間に配置するものとする（SHALL）。

#### Scenario: New Tab ボタン配置
- **GIVEN** Side Panel が開いている
- **WHEN** UI を確認する
- **THEN** Pins の下、Today Tabs の上に New Tab ボタンが表示される

### Requirement: Today Tabs セクション
Today Tabs は未整理リストとして表示し、各 Item に削除用の×ボタンを表示するものとする（SHALL）。

#### Scenario: Today Tabs リスト表示
- **GIVEN** Today Tabs に Item が存在する
- **WHEN** Today Tabs セクションを確認する
- **THEN** Item がリスト形式で表示され、各 Item に×ボタンがある

### Requirement: Folder Tree セクション
Folder Tree はフォルダ階層と保存 Item を木構造で表示するものとする（SHALL）。展開・折りたたみが可能。

#### Scenario: Folder Tree 表示
- **GIVEN** Folder と Saved Item が存在する
- **WHEN** Folder Tree セクションを確認する
- **THEN** フォルダが階層構造で表示され、クリックで展開・折りたたみできる

### Requirement: 犬アイコンのフォールバック
favicon が取得できない場合は、デフォルトの犬アイコン（SVG）を表示するものとする（SHALL）。

#### Scenario: 犬アイコン表示
- **GIVEN** Item の favicon が取得できない
- **WHEN** Item が表示される
- **THEN** favicon の代わりに犬アイコンが表示される

### Requirement: React によるUI実装
UI は React を使用して実装するものとする（SHALL）。

#### Scenario: React アプリ
- **GIVEN** Side Panel が開かれる
- **WHEN** HTML がロードされる
- **THEN** React アプリがマウントされて UI がレンダリングされる

