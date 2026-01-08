# pins Specification

## Purpose
TBD - created by archiving change add-initial-specs. Update Purpose after archive.
## Requirements
### Requirement: Pins の定義
Pins は Space ごとのお気に入り Item をアイコン列として表示する機能である。各 Space は独立した Pins リストを持つものとする（SHALL）。

#### Scenario: Pins の構造
- **GIVEN** Space が存在する
- **WHEN** Pins を参照する
- **THEN** その Space に紐づく Item ID の配列（順序付き）が取得できる

### Requirement: Pins の表示
Pins は Side Panel の上部に横並びのアイコン列として表示されるものとする（SHALL）。

#### Scenario: Pins 表示
- **GIVEN** アクティブな Space に Pins が設定されている
- **WHEN** Side Panel が表示される
- **THEN** Pins がアイコン列として表示される（favicon または犬アイコン）

### Requirement: Item を Pins に追加
ユーザーは Item を現在の Space の Pins に追加できるものとする（SHALL）。

#### Scenario: Pins 追加
- **GIVEN** Item が存在する
- **WHEN** ユーザーが Item を Pins に追加する
- **THEN** その Item が Pins リストの末尾に追加される

### Requirement: Pins から Item を削除
ユーザーは Pins から Item を削除できるものとする（SHALL）。Item 自体は削除されない。

#### Scenario: Pins 削除
- **GIVEN** Item が Pins に追加されている
- **WHEN** ユーザーが Pins から削除する
- **THEN** Pins リストからその Item が削除される
- **AND** Item 自体は引き続き存在する

### Requirement: Pins のクリック挙動
Pins をクリックすると、その Item の起点 URL を新規タブで開き、Today Tabs の先頭に追加するものとする（SHALL）。

#### Scenario: Pins クリック
- **GIVEN** Pins に Item が存在する
- **WHEN** ユーザーが Pin アイコンをクリックする
- **THEN** その Item の URL が新規タブで開かれる
- **AND** Today Tabs の先頭に追加される

### Requirement: Pins の表示順序
Pins は追加された順序で表示されるものとする（SHALL）。新しく追加された Pin は末尾に配置される。

#### Scenario: Pins の追加順序
- **GIVEN** Pins に Item A, B が順に追加されている
- **WHEN** Item C を Pins に追加する
- **THEN** Pins は A, B, C の順序で表示される

> **NOTE (MVP Scope)**: Pins の並び替え（ドラッグ＆ドロップによる順序変更）は MVP では実装しない。将来的に順序変更が必要になった場合に検討する。

