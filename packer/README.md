
## Steps

1. Create service account

    ```
export SERVICE_ACCOUNT_NAME=packer
gcloud iam service-accounts create $SERVICE_ACCOUNT_NAME \
    --display-name=$SERVICE_ACCOUNT_NAME
    ```

1. Grant permissions

    ```
export SERVICE_ACCOUNT="$SERVICE_ACCOUNT_NAME@$GOOGLE_CLOUD_PROJECT.iam.gserviceaccount.com"
gcloud projects add-iam-policy-binding $GOOGLE_CLOUD_PROJECT \
    --member=serviceAccount:$SERVICE_ACCOUNT --role=roles/compute.instanceAdmin.v1
gcloud projects add-iam-policy-binding $GOOGLE_CLOUD_PROJECT \
    --member=serviceAccount:$SERVICE_ACCOUNT --role=roles/iam.serviceAccountUser
    ```

1. Create key

    ```
export KEY_PATH="$(pwd)/$SERVICE_ACCOUNT_NAME.json"
gcloud iam service-accounts keys create $KEY_PATH \
    --iam-account $SERVICE_ACCOUNT
    ```

1. Set parameter

```
sed 's/<SERVICE_ACCOUNT_FILENAME>/$SERVICE_ACCOUNT_NAME/' packer.json
sed 's/<PROJECT_ID>/$GOOGLE_CLOUD_PROJECT/' packer.json
```

1. Run packer

```
packer validate my-template.json
packer build my-template.json
```
