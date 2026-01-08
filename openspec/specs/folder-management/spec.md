# folder-management Specification

## Purpose
TBD - created by archiving change add-initial-specs. Update Purpose after archive.
## Requirements
### Requirement: Folder の定義
Folder は Space 内の階層構造（ディレクトリ）を表す。各 Folder は親 Folder を持つことができ、木構造を形成するものとする（SHALL）。Folder には Item（Saved Items）を含めることができる。

#### Scenario: Folder の識別
- **GIVEN** システムに Folder が存在する
- **WHEN** Folder を参照する
- **THEN** 一意の ID で識別できる

#### Scenario: Folder のプロパティ
- **GIVEN** Folder が存在する
- **WHEN** Folder の情報を取得する
- **THEN** 名前（name）、所属 Space ID、親 Folder ID（ルートの場合は null）を含む

### Requirement: Folder の作成
ユーザーは Space 内に新しい Folder を作成できるものとする（SHALL）。

#### Scenario: ルートレベル Folder 作成
- **GIVEN** アクティブな Space が選択されている
- **WHEN** ユーザーが新しい Folder を作成する
- **THEN** 親 Folder を持たないルート Folder が作成される

#### Scenario: サブ Folder 作成
- **GIVEN** 親 Folder が存在する
- **WHEN** ユーザーがその Folder 内に新しい Folder を作成する
- **THEN** 親 Folder の子として新しい Folder が作成される

### Requirement: Folder の展開・折りたたみ
ユーザーは Folder Tree 内の Folder を展開・折りたたみできるものとする（SHALL）。展開状態は UI 状態として永続化される。

#### Scenario: Folder 展開
- **GIVEN** 折りたたまれた Folder が存在する
- **WHEN** ユーザーが Folder をクリックして展開する
- **THEN** 子 Folder と Item が表示される
- **AND** expandedFolderIds に追加される

#### Scenario: Folder 折りたたみ
- **GIVEN** 展開された Folder が存在する
- **WHEN** ユーザーが Folder をクリックして折りたたむ
- **THEN** 子 Folder と Item が非表示になる
- **AND** expandedFolderIds から削除される

### Requirement: Folder の削除
ユーザーは Folder を削除できるものとする（SHALL）。削除時は、その Folder 内の全ての子 Folder と Item も同時に削除される。

#### Scenario: Folder 削除
- **GIVEN** Folder が存在する
- **WHEN** ユーザーが Folder を削除する
- **THEN** その Folder と全ての子孫（Folder/Item）が削除される

### Requirement: Folder の名前変更
ユーザーは Folder の名前を変更できるものとする（SHALL）。

#### Scenario: Folder 名前変更
- **GIVEN** Folder が存在する
- **WHEN** ユーザーが新しい名前を入力して確定する
- **THEN** Folder の名前が更新される

### Requirement: Folder Tree の構築
システムは現在の Space 内の Folder を木構造（Folder Tree）として組み立てて表示するものとする（SHALL）。

#### Scenario: Folder Tree 表示
- **GIVEN** アクティブな Space が選択されている
- **WHEN** Side Panel が表示される
- **THEN** その Space に属する Folder が階層構造で表示される

