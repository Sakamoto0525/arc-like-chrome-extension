# space-management Specification

## Purpose
TBD - created by archiving change add-initial-specs. Update Purpose after archive.
## Requirements
### Requirement: Space の定義
Space は作業コンテキスト（Work / Private など）を表す最上位の論理単位である。システムは複数の Space を管理し、各 Space は独立した Folder 構造と Pins を持つものとする（SHALL）。

#### Scenario: Space の識別
- **GIVEN** システムに Space が存在する
- **WHEN** Space を参照する
- **THEN** 一意の ID で識別できる

#### Scenario: Space のプロパティ
- **GIVEN** Space が存在する
- **WHEN** Space の情報を取得する
- **THEN** 名前（name）と作成日時を含む

### Requirement: Space の作成
ユーザーは新しい Space を作成できるものとする（SHALL）。

#### Scenario: 新規 Space 作成
- **GIVEN** ユーザーが Side Panel を開いている
- **WHEN** 新しい Space 名を入力して作成を実行する
- **THEN** 新しい Space が作成され、空の Folder 構造と Pins リストが初期化される

### Requirement: Space の切り替え
ユーザーは Header の Space Switcher を使用して、アクティブな Space を切り替えられるものとする（SHALL）。

#### Scenario: Space 切り替え
- **GIVEN** 複数の Space が存在する
- **WHEN** ユーザーが別の Space を選択する
- **THEN** UI が選択された Space の内容（Pins / Today Tabs / Folders）を表示する
- **AND** activeSpaceId が更新される

### Requirement: Space の削除
ユーザーは Space を削除できるものとする（SHALL）。削除時は、その Space に属する全ての Folder、Item、Pins も同時に削除される。

#### Scenario: Space 削除
- **GIVEN** 複数の Space が存在する
- **WHEN** ユーザーが Space を削除する
- **THEN** その Space と関連する全データが削除される
- **AND** 別の Space が自動的にアクティブになる

### Requirement: Space の名前変更
ユーザーは Space の名前を変更できるものとする（SHALL）。

#### Scenario: Space 名前変更
- **GIVEN** Space が存在する
- **WHEN** ユーザーが新しい名前を入力して確定する
- **THEN** Space の名前が更新される

