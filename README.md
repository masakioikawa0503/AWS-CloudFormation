# AWS-CloudFormation

AWSのCloudFormation以下の自動化設定を記載しました
（一応AWSアカウントを取得後の無料利用枠12か月の範囲で収まるように設定ファイルを記載しました）
（設定の詳細はAWS公式ドキュメントを参照⇒https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/Welcome.html）

・VPC
  10.0.0.0/16
  
・Subnet
  10.0.10.0/24(Public-1a)　⇒ここにEC2(webサーバ)を立てる
  10.0.11.0/24(Public-1c)　⇒本設定ではここにEC2を立てないが、必要に応じて立てることも可能
  10.0.20.0/24(Private-1a)
  10.0.21.0/24(Private-1c)
  
・IGW
  インターネットGW作成
  作成したIGWとVPCを紐づけする
  
・RouteTable／Route - The Public Subnet to the IGW／SubnetRouteTableAssociation - PublicSubnetRouteTable
  IGWとつなぐルートテーブルを作成
  作成したルートテーブルにIGWへ向かう設定を記載
  サブネット（Public-1a）と作成したルートテーブルを紐づける
 
・Security Group
  web
    in:   宛先22,80,3000に対してポート許可
    out:  全て許可

  RDS
    in:   宛先3306に対して、Public-1aからのアクセスのみ許可
    out:  全て許可
  
  ELB(ALB)
    in:   webと同じ 
    out:  webと同じ

・EC2
  秘密鍵はパラメータで渡すようにしました。
  AMIはAmazonLinux2の最新erを自動的に公開サイトから読み取ってくるように設定
  （上記はParameterに記載済）
  
  Public-1aに配置
  インスタンスタイプはt2.micro
  
・RDS
  Private-1aに配置
  インスタンスタイプはdb.t2.micro
  Mysqlのverは8.0.15
  
・ELB
  本設定ファイルでは試験的に1台のEC2(web)に対して設定ファイルを記載
