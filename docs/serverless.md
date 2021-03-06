<strong style="color: red; ">
※ 現在学習中で間違った内容を記載している可能性が大いにあります。もし間違いがありましたら、遠慮なくご指摘くださいませ。
</strong>

# サーバレス

サーバレスの説明の前に疎結合化について理解する必要がある。

疎結合化とは、システムを構成するコンポーネント同士がお互いに依存しあわない状態をいう。メリットは、コンポーネントが故障したときなどに、別のコンポーネントを生成しシステムとしての処理を継続できるようになること。

疎結合向けのAWSサービスは

- ELB
	- サーバー間のトラフィック調整と連携をELBを起点に結ぶことで、疎結合化を実現
- SQS
	- SQSキューイングによる通信でインスタンス関連携を結ぶことで疎結合化を実現
- SNS
	- SNSアプリケーション間通信でインスタンス間連携を結ぶことで疎結合化を実現
- Lambda
	- サーバーインスタンスではなくLambdaによるトリガー処理で連携することで疎結合化を実現

その他、SES/DynamoDB（サーバレス化したデータを処理）/APIGateway（サーバレス化したアーキテクチャを繋げる）/Cognitoがある。

サーバレスとは、EC2インスタンスなどのサーバーでは無く、Lambdaなどのコンピュートサービスによるシステム構成をできるかぎりマイクロサービス化して、使用するという考え方。サーバレス化して、マイクロサービス間をAPI通信することで、低コスト疎結合化を実現することができる。