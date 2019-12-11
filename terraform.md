# Terraform

## Related links

- https://learn.hashicorp.com/terraform/getting-started/build
- https://www.terraform.io/docs/configuration/index.html

## Configration

### Key factors

#### Resources

- 作るもの。Providerに依存

#### Module

- リソースのグループ。リソースをまとめる

```
resource "aws_vpc" "main" {
  cidr_block = var.base_cidr_block
}

<BLOCK TYPE> "<BLOCK LABEL>" "<BLOCK LABEL>" {
  # Block body
  <IDENTIFIER> = <EXPRESSION> # Argument
}
```

#### Code Organization

- 拡張子は`.tf`
- Terraformを走らせるところがWorking directory, 子ディレクトリも参照対象
- 宣言順序は自動的にTerraformが依存関係を解決するので基本関係ない（provisionerは関係する？）

#### Terraform CLI vs. Providers

- CLIは設定シンタックスを理解し、`providers`プラグインに仕事を任せる
- resourcesはprovidersに依存しているが同じシンタックスで設定をかける

### Resource

#### Resource Syntax

resource リソースタイプ　ローカル名

```
resource "aws_instance" "web" {
  ami           = "ami-a1b2c3d4"
  instance_type = "t2.micro"
}
```

- ローカル名は同一モジュール内で参照可（スコープで閉じられている）

#### Resource Types and Arguments

- Resource blockのbodyに引数を記載
- 全リソースで共有される`メタ属性`がある
  - https://www.terraform.io/docs/configuration/resources.html#meta-arguments

#### Documentation for Resource Types

基本的シンタックス習得後は各Providerのドキュメントを参照
https://www.terraform.io/docs/providers/index.html

#### Resource Behavior

- resource blockで定義されたリソースは、実際に生成されたリソース際それを特定するためのIDを`state`に格納する
- IDはリソースの作成、更新、削除に使用される
- Terraformは生成されたリソースの設定と設定ファイルの設定（属性値）を比較し必要であれば更新する
- 作成、更新、削除の意味はそれぞれのリソースによって異なる

#### Resource Dependencies

- ほとんどのリソース依存関係は自動的に処理される
- 依存関係がないリソースは並列処理される
- Terraformが解析できない暗黙の依存関係には`depends_on`で明示する（配列で参照名を列挙）
- `depends_on`は最終手段。なぜ必要なのかコメントする

#### Meta-Arguments

##### depends_on

##### count

- `count = 4` のようにリソースを複数作成できる
- `conut.index`で個別のインスタンスを参照可能（i.e. aws_instance.server[0]）
- 式を使用する場合、リソースが作成されるまで参照不可（動的IDなどと同様）

##### for_each

- countを拡張したようなもの
- 各インスタンスに引数を渡せる

```
resource "azurerm_resource_group" "rg" {
  for_each = {
    a_group = "eastus"
    another_group = "westus2"
  }
  name     = each.key
  location = each.value
}
```

- a = "b" の場合、`each.key`で"a"が`each.value`で"b"が参照可能
- set（配列）が与えられた場合は`each.key`と`each.value`は同じ

##### provider

- デフォルトではリソース名のアンダーバーで区切った最初のキーワードをproviderのデフォルトの設定として使用する
  - "google_compute_instance"だったら"google"
- `provider`属性には式は使用できない（依存関係を解決する前に必要なため）

```
# default configuration
provider "google" {
  region = "us-central1"
}

# alternative, aliased configuration
provider "google" {
  alias  = "europe"
  region = "europe-west1"
}

resource "google_compute_instance" "example" {
  # This "provider" meta-argument selects the google provider
  # configuration whose alias is "europe", rather than the
  # default configuration.
  provider = google.europe

  # ...
}
```

##### lifecycle

- デフォルトの作成、更新、削除の処理内容をカスタマイズできる
- `create_before_destroy`は必須リソースなどで有効

##### provisioner / connection

- 作成後起動するための準備（プロビジョン）を必要とするリソースで使用

#### Local-only Resources

- 認証情報（private keysやTLS certification）も内部リソースとしてstateに保存される

#### Operation Timeouts

- いくつかのリソースで利用可
