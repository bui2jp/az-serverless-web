# az-serverless-web

https://github.com/bui2jp/serverless-reference-implementation

```
export LOCATION=japaneast
export RESOURCEGROUP=az-learn-2022
export APPNAME=my-test-fun # Cannot be more than 6 characters
export APP_INSIGHTS_LOCATION=my-test-appi
export COSMOSDB_DATABASE_NAME=my-test-db
export COSMOSDB_DATABASE_COL=my-test-col

az group create -n $RESOURCEGROUP -l $LOCATION
```
# Functions

```
az storage account create -n mystorage202208070807 -g $RESOURCEGROUP -l $LOCATION --sku Standard_LRS --kind StorageV2
az functionapp create -g $RESOURCEGROUP --consumption-plan-location $LOCATION --runtime node --runtime-version 14 --functions-version 4 --name my-func-202208070807 --storage-account mystorage202208070807 

func azure functionapp publish my-func-202208070807 --publish-local-settings -y
```

# Azure API Management
apim
```
az apim create --name my-example2-apim20220807 --resource-group $RESOURCEGROUP --publisher-name my-apim-example --publisher-email chikutaku.reiwa.2020@gmail.com  --no-wait
```

## API Management の　　Inbound ポリシーの例
```
<policies>
    <inbound>
        <validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="My Unauthorized. Access token is missing or invalid.">
            <openid-config url="https://login.microsoftonline.com/<tenant-id>/v2.0/.well-known/openid-configuration" />
            <required-claims>
                <claim name="aud">
                    <value>d89a47ad-2ad2-4367-8fc0-c40d386ea3d9</value>
                </claim>
            </required-claims>
        </validate-jwt>
        <cors allow-credentials="false">
            <allowed-origins>
                <origin>*</origin>
            </allowed-origins>
            <allowed-methods preflight-result-max-age="300">
                <method>*</method>
            </allowed-methods>
            <allowed-headers>
                <header>*</header>
            </allowed-headers>
        </cors>
        <base />
    </inbound>
    <backend>
        <base />
    </backend>
    <outbound>
        <base />
    </outbound>
    <on-error>
        <base />
    </on-error>
</policies>
```

```
curl https://my-example2-apim20220807.azure-api.net/HttpTrigger -H 'Authorization: xxxxxxxxxxxx'
```


