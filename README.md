# `Docker`+`Terraform`+`CircleCI`+`ECR`+`ECS` で `CI/CD` ハンズオン

## クライアント環境構築(10～20分)
- GCEのDebianで作業する
  - terraform Docker Containerをpull, run
    - $ sudo docker pull hashicorp/terraform:full
    - $ sudo docker run -it --entrypoint /bin/bash hashicorp/terraform:full
    - $ terraform version
    - exit(終了)、デタッチ(切断)とアタッチ(接続)の話

## AWS ECR/ECS へのデプロイ(40～50分)
- https://circleci.com/docs/ja/2.0/ecs-ecr/
  - サンプルプロジェクトのクローンを作成
    - $ cd src/github.com
    - $ git clone https://github.com/CircleCI-Public/circleci-demo-aws-ecs-ecr
    - $ cd circleci-demo-aws-ecs-ecr/terraform_setup
  - AWS 変数の実際の値で ~/terraform_setup/terraform.tfvars を更新
    - $ vi terraform.tfvars 
  - AWSリソースを作成
    - $ terraform init
      - エラーが発生するので解消する
    - $ terraform plan
      - エラーが発生するので解消する
    - $ terraform apply
      - CloudFormation, VPC, ロードバランサー, ESC, ESRあたりが作成されているところを画面から見てみる
      - 完了したらロードバランサーのDNS名でアクセスしてみる
  - GitHubの自分のリポジトリにpush
    - $ cd ..
    - $ vi .gitignore
      - terraform_setup/terraform.tfvars
      - ※認証情報を含むため除外する
    - $ git config --global user.email "メールアドレス"
    - $ git config --global user.name "ユーザー名"
    - $ git remote -v
    - $ git push ユーザー名 master
    - $ git add .
    - $ git commit -m "first commit"
    - $ git remote add ユーザー名 https://github.com/ユーザー名/circleci-demo-aws-ecs-ecr.git
    - $ git push ユーザー名 master
  - CircleCIでAdd Project(環境変数を登録する前にいったんJobを実行し、FAILEDにする)
  - CircleCI 環境変数を設定する
    - AWS_ACCESS_KEY_ID
    - AWS_SECRET_ACCESS_KEY
    - AWS_DEFAULT_REGION
    - AWS_ACCOUNT_ID
    - AWS_RESOURCE_NAME_PREFIX
  - 適当にpushしてCircleCIのJobを実行させ、SUCCESSにする
    - 完了したらロードバランサーのDNS名でアクセスしてみる