<strong style="color: red; ">
※ 現在学習中で間違った内容を記載している可能性が大いにあります。もし間違いがありましたら、遠慮なくご指摘くださいませ。
</strong>

# CloudFormation

CloudFormationは、YML or JSON 形式で書かれたテンプレートファイルを読み込んで使用する。テンプレートファイルにはAWSリソースの構成を記述することができ、一括して構築するテンプレートを作成することができる。

テンプレートファイルによって作成されたAWSリソース群をスタックと呼ぶ。

### 機能
- **変更セット**
    - スタックの更新、実行前に変更による影響を確認することができる機能
    - 意図しないリソースの削除などのミスを防ぐことができる
- **ドリフト**
    - 前回実行したテンプレートとの差分を確認することができる
    - 意図しないリソースの削除などのミスを防ぐことができる
- **スタックセット**
    - 複数のAWSアカウントと複数のリージョンに対してスタックを作成できる機能
- スタック間のリソース参照機能
    - 後述のクロススタックのさいなどに、すでに作成してあるテンプレートの情報を参照して、すでに作成してあるリソースを使用する機能

### テンプレートの構成
[公式ページ](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/template-anatomy.html)より引用。

```yaml

AWSTemplateFormatVersion: "version date"

Description:
  String

Metadata:
  template metadata

Parameters:
  set of parameters

Mappings:
  set of mappings

Conditions:
  set of conditions

Transform:
  set of transforms

Resources:
  set of resources

Outputs:
  set of outputs

```

- **AWSTemplateFormatVersion**（任意）
    - AWSCloudFormationのテンプレートバージョンの日付
- **Description**（オプション）
    - テンプレートがどういったものかを記述する
- **Metadata**（オプション）
    - テンプレートに関する追加情報を提供するオブジェクト
- **Parameters**（任意）
    - スタック更新や作成時に、テンプレートに渡すことができる値
- **Mappings**（任意）
    - キーと関連する値のマッピング
- **Conditions**（オプション）
    - スタックが本番用かテスト用かに依存する、条件付きのリソースを作成できる
- **Transform**（オプション）
    - Lambda関数などの構文と、その処理方法を定義する
- **Resources**（必須）
    - テンプレートのメイン項目で、スタックを構成するリソースを記述する
- **Outputs**（任意）
    - テンプレートの実行の際に、表示される返り値を設定する。クロススタックの際（import）に利用する。

#### Resourcesを少し深ぼる
[公式ページ](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/resources-section-structure.html)より引用。

```yaml
Resources:
  MyEC2Instance:
    Type: "AWS::EC2::Instance"
    Properties:
      ImageId: "ami-0ff8a91507f77f867"
```

- LogicalID（論理ID）
    - 英数字で、テンプレート内で一意である必要がある
- リソースタイプ
    - 宣言しているリソースのタイプの識別
- リソースプロパティ
    - リソースに対して追加できるオプション、AMI IDなど

#### クロススタック
CloudFormationで作成されるスタックは、一つのテンプレートに全てのリソースを記述するのではなく、大まかなグループごとの階層に分けてスタックを作成する。階層ごとに作成されたスタックは、お互いに参照して紐付けて利用する。この方法をクロススタックと呼ぶ。

- ネットワーク階層
    - VPC/サブネット/エンドポイント/ルートテーブル/内部DNS(Route53)/IGW/NGW/VGW
- セキュリティグループ階層
    - セキュリティグループ/IAMポリシー/IAMグループ/IAMロール
- アプリケーション階層
    - EC2/RDS/ELB/SQSキュー/ActiveDirectory/CICD