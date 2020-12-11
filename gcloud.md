# Google cloud platform commands / tips

<!-- TOC -->

- [Google cloud platform commands / tips](#google-cloud-platform-commands--tips)
    - [Get GKE cluster credentials](#get-gke-cluster-credentials)

<!-- /TOC -->

## Get GKE cluster credentials

```
gcloud container clusters get-credentials <CLUSTER_NAME> \
    --region <REGION> \
    --project <PROJECT_ID>
```

