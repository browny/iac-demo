
## Steps (Running in Cloud Shell)

```
gcloud deployment-manager deployments create vm-dpm --config config.yml --preview
gcloud deployment-manager deployments update vm-dpm
gcloud deployment-manager deployments delete vm-dpm
```
