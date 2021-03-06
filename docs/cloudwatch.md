<strong style="color: red; ">
※ 現在学習中で間違った内容を記載している可能性が大いにあります。もし間違いがありましたら、遠慮なくご指摘くださいませ。
</strong>

# CloudWatch

単一のプラットフォーム（CloudWatch）で、AWSとオンプレのサーバーで実行されるすべてのAWSリソースのデータを収集、監視することができる。また、事前に設定されたしきい値をもとに通知を設定することもでき、超過料金の監視にも役立つ。また、しきい値をもとにアクションを設定することもできる。

### 主なサービス
- **CloudWatchMetrics**
メトリクスとは、システムのパフォーマンスに関するデータのこと。デフォルトで多くのAWSサービスがリソースの無料メトリクスを提供している。これらをモニタリングすることができる。詳細なモニタリングや詳細な設定を行うことで有料枠となる場合がある。メトリクスデータは15ヶ月間保持される。

- **CloudWatchLogs**
AWSで使用中の全てのシステム、アプリケーション、サービスからのログを一元管理することができる。エラーログなども時間順にみることができる。

- **CloudWatchEvents**
AWSリソースに対するイベントに対して、ルールをもとにアクションを実行することができる。

- **CloudWatchAlarm**
設定されたしきい値をもとにアラーム通知を行う。
- 正常
    - しきい値を下回っている
- 異常
    - 設定されたしきい値を上回っている（Alarm）
- 判定不能
    - データ不足で判定できない