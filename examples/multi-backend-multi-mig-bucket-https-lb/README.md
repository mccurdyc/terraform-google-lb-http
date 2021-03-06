# HTTPS Content-Based Load Balancer Example

[![button](http://gstatic.com/cloudssh/images/open-btn.png)](https://console.cloud.google.com/cloudshell/open?git_repo=https://github.com/GoogleCloudPlatform/terraform-google-lb-http&working_dir=examples/https-content&page=shell&tutorial=README.md)

This example creates an HTTPS load balancer to forward traffic to a custom URL map. The URL map sends traffic to the region closest to you with static assets being served from a Cloud Storage bucket. The TLS key and certificate is generated by Terraform using the [TLS provider](https://www.terraform.io/docs/providers/tls/index.html).

**Figure 1.** *diagram of Google Cloud resources*

![architecture diagram](https://raw.githubusercontent.com/GoogleCloudPlatform/terraform-google-lb-http/master/examples/multi-back-multi-mig-bucket-https-lb/diagram.png)

## Change to the example directory

```
[[ `basename $PWD` != multi-backend-multi-mig-bucket-https-lb ]] && cd examples/multi-backend-multi-mig-bucket-https-lb/
```

## Install Terraform

1. Install Terraform if it is not already installed (visit [terraform.io](https://terraform.io) for other distributions):

## Set up the environment

1. Set the project, replace `YOUR_PROJECT` with your project ID:

```
PROJECT=YOUR_PROJECT
```

```
gcloud config set project ${PROJECT}
```

2. Configure the environment for Terraform:

```
[[ $CLOUD_SHELL ]] || gcloud auth application-default login
export GOOGLE_PROJECT=$(gcloud config get-value project)
```

## Run Terraform

```
terraform init
terraform apply
```

## Testing

1. Open URL of load balancer in browser:

```
echo http://$(terraform output load-balancer-ip)
```

> You should see the GCP logo and instance details from the group closest to your geographical region.

2. Open URL to route mapped to us-west1 instance group:

```
echo https://${EXTERNAL_IP}/group1/
```

> You should see the GCP logo and instance details from the group in us-west1.

3. Open URL to route mapped to us-central1 instance group:

```
echo https://${EXTERNAL_IP}/group2/
```

> You should see the GCP logo and instance details from the group in us-central1.

4. Open URL to route mapped to us-east1 instance group:

```
echo https://${EXTERNAL_IP}/group3/
```

> You should see the GCP logo and instance details from the group in us-east1.

## Cleanup

1. Remove all resources created by terraform:

```
terraform destroy
```

<!-- BEGINNING OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|:----:|:-----:|:-----:|
| group1\_region |  | string | `"us-west1"` | no |
| group1\_zone |  | string | `"us-west1-a"` | no |
| group2\_region |  | string | `"us-central1"` | no |
| group2\_zone |  | string | `"us-central1-f"` | no |
| group3\_region |  | string | `"us-east1"` | no |
| group3\_zone |  | string | `"us-east1-b"` | no |
| network\_name |  | string | `"ml-bk-ml-mig-bkt-s-lb"` | no |
| project |  | string | n/a | yes |

## Outputs

| Name | Description |
|------|-------------|
| asset-url |  |
| group1\_region |  |
| group2\_region |  |
| group3\_region |  |
| load-balancer-ip |  |

<!-- END OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
