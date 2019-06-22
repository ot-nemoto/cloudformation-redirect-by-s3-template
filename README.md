# cloudformation-redirect-by-s3-template

## 概要

- S3の `Static website hosting` 機能を利用してリダイレクト環境を構築する
- 上記だけではHTTPSに対応できないので、CloudFrontを組み合わせHTTPSによるリクエストのリダイレクトにも対応させる
- CloudFrontでは特定のリージョンのACMしか利用できないため、以下ではバージニア北部に環境を構築する

## システム構成

![システム構成](https://raw.githubusercontent.com/ot-nemoto/cloudformation-redirect-by-s3-template/master/cloudformation-redirect-by-s3-template.png)

## 前提条件

- ホストゾーン(例の場合は`example.com`)は Route53 登録済みであること

## 構築手順

```sh
# 例
HOSTED_ZONE_NAME=example.com
ORIGIN_HOSTNAME=old.example.com
REDIRECT_HOSTNAME=new.example.com

aws cloudformation create-stack \
  --region us-east-1 \
  --stack-name redirect-${ORIGIN_HOSTNAME//./-}-to-${REDIRECT_HOSTNAME//./-} \
  --parameters ParameterKey=HostedZoneName,ParameterValue=${HOSTED_ZONE_NAME} \
               ParameterKey=OriginHostName,ParameterValue=${ORIGIN_HOSTNAME} \
               ParameterKey=RedirectHostName,ParameterValue=${REDIRECT_HOSTNAME} \
  --template-body file://template.yml
```
