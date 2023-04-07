# AWS Distro for OpenTelemetry in EKS Fargate Helm Chart

This Helm chart installs AWS Distro for OpenTelemetry in an EKS Fargate cluster.  
The resources created are the same as in this documentation for [AWS Collector on EKS Fargate](https://aws-otel.github.io/docs/getting-started/container-insights/eks-fargate), plus a service account that 

## Prerequisites

- An EKS Fargate cluster
- A Fargate profile for the namespace this chart is installed into (default is fargate-container-insights)
- Service Account with IAM Role granting CloudWatch permissions
    -   e.g. A role with the `arn:aws:iam::aws:policy/CloudWatchAgentServerPolicy` managed policy
- Helm 3
- Kubernetes version 1.21 or higher

## Installation

1. Add the AWS Distro for OpenTelemetry Helm repository to Helm:

        helm repo add adot-fargate https://github.com/cjh-cloud/adot-fargate
        helm repo update  

2. Install the AWS Distro for OpenTelemetry Helm chart:

        helm install adot-fargate cjh-cloud/adot-fargate \
        --set clusterName=my-cluster \
        --set region=ap-southeast-2 \
        --set serviceAccount.iamRoleArn=arn:aws:iam::[ACCOUNT ID]:role/adot-role

    *OR* Install via repo:

        helm upgrade --install aws-distro-open-telemetry ./ \
        --set clusterName=my-cluster \
        --set region=ap-southeast-2 \
        --set serviceAccount.iamRoleArn=arn:aws:iam::[ACCOUNT ID]:role/adot-role

3. Verify that the AWS Distro for OpenTelemetry deployment is running:

        kubectl get pods -n fargate-container-insights

    The output should show a pod with the name `adot-collector-0`.  
    View "Container Insights" under AWS CloudWatch to view EKS Cluster metrics.

## Configuration

The AWS Distro for OpenTelemetry Helm chart can be configured using the following options:

| Parameter | Description | Default |
| --- | --- | --- |
| `clusterName` | name of the AWS EKS cluster to deploy to | None |
| `region` | the AWS region to deploy to | None |
| `serviceAccount.create` | If `true`, create a new service account | `true` |
| `serviceAccount.iamRoleArn` | Arn of the AWS IAM role to attach to the service account  | None |
| `image.repository` | The ECR repository that contains the AWS Distro for OpenTelemetry Docker image | `amazon/aws-otel-collector` |
| `image.tag` | The tag of the AWS Distro for OpenTelemetry Docker image | `v0.27.1` |
| `namespace` | Kubernetes namespace to install the AWS Distro for OpenTelemetry resources to | `fargate-container-insights` |
