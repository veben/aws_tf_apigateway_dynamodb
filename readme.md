# API Gateway + Dynamodb

<img src="apigateway_dynamodb.drawio.svg" alt="apigateway_dynamodb.drawio.svg" style="width:500px;height:auto;">

## 🧪 Testing it with **Gitpod**
[![Open in Gitpod](https://img.shields.io/badge/Gitpod-ready--to--code-blue?logo=gitpod)](https://gitpod.io/#https://github.com/veben/aws_tf_apigateway_dynamodb)

## 🧪 Testing it without Gitpod
### 1. 📝 Preparation
- Follow [installations](https://github.com/veben/aws_terraform_snippets/blob/main/readme.md)
- Choose **Localstack hosting** and follow the different steps
- You can also choose **Cloud hosting** and use `terraform` instead of `tflocal`

### 2. 🏃 Running
> You can follow this steps or directly launch the script `./bin/run.sh`

Now that your infrastructure is deployed on LocalStack, test its functionality by:
- Recovering the API key from terraform output
```sh
API_KEY=$(tflocal output -json | jq -r .apigw_key.value)
```
- Recovering the API id
```sh
REST_API=$(aws apigateway --endpoint-url=http://localhost:4566 get-rest-apis | jq -r '.items[0].id')
```
- Inserting data in DynamoDB via API:
```sh
curl $REST_API.execute-api.localhost.localstack.cloud:4566/v1/pets -H "x-api-key: ${API_KEY}" -H 'Content-Type: application/json' --request POST --data-raw '{ "PetType": "dog", "PetName": "tito", "PetPrice": 250 }'
```
- Requesting the inserted data
```sh
curl -H "x-api-key: ${API_KEY}" --request GET $REST_API.execute-api.localhost.localstack.cloud:4566/v1/pets/dog
```

### 3. 🚿 Cleaning up
```sh
tflocal destroy --auto-approve
```