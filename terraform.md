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
