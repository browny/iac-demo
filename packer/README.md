
## Steps (Running in Cloud Shell)

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
    sed 's/<SERVICE_ACCOUNT_FILENAME>/'"$SERVICE_ACCOUNT_NAME.json"'/g;s/<PROJECT_ID>/'"$GOOGLE_CLOUD_PROJECT"'/g' tmp > config.json
    ```

1. Install packer

    ```
    curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
    sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
    sudo apt-get update && sudo apt-get install packer
    ```

1. Run packer

    ```
    packer validate config.json
    packer build config.json
    ```
