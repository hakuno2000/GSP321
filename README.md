# [Set Up and Configure a Cloud Environment in Google Cloud: Challenge Lab](https://www.cloudskillsboost.google/focuses/10603?parent=catalog)

## Environment
![Environment](https://cdn.qwiklabs.com/UE5MydlafU0QvN7zdaOLo%2BVxvETvmuPJh%2B9kZxQnOzE%3D)

## Task 1. Create development VPC manually
```
gcloud compute networks create griffin-dev-vpc --subnet-mode=custom
gcloud compute networks subnets create griffin-dev-wp --network=griffin-dev-vpc --region=us-east1 --range=192.168.16.0/20
gcloud compute networks subnets create griffin-dev-mgmt --network=griffin-dev-vpc --region=us-east1 --range=192.168.32.0/20
```

## Task 2. Create production VPC manually
```
gcloud compute networks create griffin-prod-vpc --subnet-mode=custom
gcloud compute networks subnets create griffin-prod-wp --network=griffin-prod-vpc --region=us-east1 --range=192.168.48.0/20
gcloud compute networks subnets create griffin-prod-mgmt --network=griffin-prod-vpc --region=us-east1 --range=192.168.64.0/20
```

## Task 3. Create bastion host
```
gcloud compute instances create bastion-host --zone=us-east1-b --machine-type=n1-standard-1 \
--network-interface subnet=griffin-dev-mgmt \
--network-interface subnet=griffin-prod-mgmt
```

## Task 4. Create and configure Cloud SQL Instance
```
gcloud sql instances create griffin-dev-db --region=us-east1
```

## Task 5. Create Kubernetes cluster
```
gcloud container clusters create griffin-dev --subnetwork=griffin-dev-wp --zone=us-east1-b
gcloud container clusters get-credentials griffin-dev --zone=us-east1-b
```

## Task 6. Prepare the Kubernetes cluster
```
gsutil cp gs://cloud-training/gsp321/wp-k8s .
nano wp-env.yaml
kubectl apply -f wp-env.yaml
```

```
gcloud iam service-accounts keys create key.json \
    --iam-account=cloud-sql-proxy@qwiklabs-gcp-02-11ea6d3b33f6.iam.gserviceaccount.com
kubectl create secret generic cloudsql-instance-credentials \
    --from-file key.json
```

## Task 7. Create a WordPress deployment
```
nano wp-deployment.yaml
kubectl apply -f wp-service.yaml
```

## Task 8. Enable monitoring
## Task 9. Provide access for an additional engineer
Do these tasks in console, can't finish from CLI.
