# CDKでFargateを構築

## イメージ作成からecsへのデプロイまで

### ローカルでイメージ作成と動作
./appフォルダにて
> npm install

> docker build . -t node-web-app

> docker run -p 80:80 -d node-web-app

### ECRへDockerイメージをプッシュ

ECRに対してDockerクライアントを認証
> aws ecr get-login-password --region ap-northeast-1 | docker login --username AWS --password-stdin <AWSACCOUNTID>.dkr.ecr.ap-northeast-1.amazonaws.com

ECRリポジトリの作成、リポジトリ名は **sample-node-app**

> aws ecr create-repository --repository-name sample-node-app --image-scanning-configuration scanOnPush=true --region ap-northeast-1

IMAGE IDの確認
> docker image ls

イメージにECR用のタグを付与
> docker tag <ImageTagID> <AWSACCOUNTID>.dkr.ecr.ap-northeast-1.amazonaws.com/sample-node-app:latest

イメージのプッシュ
> docker push <AWSACCOUNTID>.dkr.ecr.ap-northeast-1.amazonaws.com/sample-node-app:latest

### dockerイメージを更新する場合
Dockerイメージの再ビルド
> docker build -t node-web-app:latest .

タグを再度付与
> docker tag <ImageTagID> <AWSACCOUNTID>.dkr.ecr.ap-northeast-1.amazonaws.com/sample-node-app:latest

イメージのプッシュ
> docker push <AWSACCOUNTID>.dkr.ecr.ap-northeast-1.amazonaws.com/sample-node-app:latest

## CDKでFargateを構築
lib/cdk-fargate-sample-stack.ts参照

### 参考
https://zenn.dev/hirokisakabe/articles/73d7d30a0e2ec8