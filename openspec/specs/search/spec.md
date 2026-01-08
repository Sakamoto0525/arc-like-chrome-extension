# search Specification

## Purpose
TBD - created by archiving change add-initial-specs. Update Purpose after archive.
## Requirements
### Requirement: 検索入力の常時表示
Side Panel の上部に検索入力フィールドを常時表示するものとする（SHALL）。

#### Scenario: 検索入力表示
- **GIVEN** Side Panel が開いている
- **WHEN** ユーザーが Side Panel を見る
- **THEN** 検索入力フィールドが常に表示されている

### Requirement: 検索対象
検索は現在の Space 内の全 Item（Today Tabs + Saved Items）を対象とするものとする（SHALL）。検索対象は Item のタイトルと URL。

#### Scenario: 検索対象範囲
- **GIVEN** アクティブな Space に Item が存在する
- **WHEN** ユーザーが検索クエリを入力する
- **THEN** Today Tabs と Folder 内の全 Item が検索対象となる

### Requirement: 検索フィルタリング
ユーザーが検索クエリを入力すると、マッチする Item のみが表示されるものとする（SHALL）。

#### Scenario: 検索実行
- **GIVEN** 複数の Item が存在する
- **WHEN** ユーザーが "example" と入力する
- **THEN** タイトルまたは URL に "example" を含む Item のみ表示される

#### Scenario: 検索クリア
- **GIVEN** 検索フィルタが適用されている
- **WHEN** ユーザーが検索入力をクリアする
- **THEN** 全ての Item が通常通り表示される

### Requirement: 検索結果のクリック挙動
検索結果の Item をクリックすると、その URL を新規タブで開き、Today Tabs の先頭に追加するものとする（SHALL）。

#### Scenario: 検索結果クリック
- **GIVEN** 検索結果に Item が表示されている
- **WHEN** ユーザーが Item をクリックする
- **THEN** その Item の URL が新規タブで開かれる
- **AND** Today Tabs の先頭に追加される

### Requirement: インクリメンタル検索
検索は入力に応じてリアルタイムでフィルタリングを行うものとする（SHALL）。

#### Scenario: インクリメンタル検索
- **GIVEN** 検索入力フィールドにフォーカスがある
- **WHEN** ユーザーが文字を入力する
- **THEN** 入力ごとにフィルタ結果が即座に更新される

