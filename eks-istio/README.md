# EKS ISTIO Installation with TLS and Cert Manager

This directory contains additional resources and code samples to support users who want to install the application on Amazon EKS with ISTIO, utilizing TLS encryption and Cert Manager for certificate management.

## Overview

The code and configurations provided here go beyond the scope of the associated blog post. While the blog post covers the fundamental aspects of the application setup, the contents of this directory cater to users interested in deploying the application on EKS with ISTIO while enabling TLS with Cert Manager.

## Contents

- **/iam**: Contains IAM (Identity and Access Management) resources for AWS Load Balancer controller.

  - `/iam/aws-lb-controller.json`: AWS Load Balancer controller IAM policy.
  - `/iam/lb-service-account.yml`: Kubernetes Service Account for AWS Load Balancer controller.
  - `/iam/load-balancer-role-trust-policy.json`: IAM Role trust policy for the Load Balancer controller.

- **/service-mesh**: Includes resources related to the service mesh.

  - `/service-mesh/gateway-operator.yml`: Kubernetes manifest for deploying ISTIO gateway for the application.

- **/workload**: Contains resources related to the application workload.

  - `/workload/aws-lb-ingess.yml`: Kubernetes Ingress definition for AWS Load Balancer.
  - `/workload/cert-man.yml`: Kubernetes manifest for Cert Manager.
  - `/workload/gateway.yml`: ISTIO Gateway definition.
  - `/workload/virtual-service.yml`: ISTIO VirtualService definition.

## Prerequisites

Make sure you have the following prerequisites before using the resources in this directory:

- An active Amazon EKS cluster.
- ISTIO installed on the EKS cluster.
- Cert Manager installed on the EKS cluster.

### Install EKS with EKSCTL

```
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig
metadata:
  name: album-app
  region: ap-southeast-2
  version: "1.27"


availabilityZones:
- ap-southeast-2a
- ap-southeast-2b
- ap-southeast-2c

vpc:
  cidr: 10.42.0.0/16
  clusterEndpoints:
    privateAccess: true
    publicAccess: true

iam:
  withOIDC: true
  
addons:
- name: vpc-cni
  version: v1.12.5-eksbuild.2
  configurationValues: "{\"env\":{\"ENABLE_PREFIX_DELEGATION\":\"true\", \"ENABLE_POD_ENI\":\"true\", \"POD_SECURITY_GROUP_ENFORCING_MODE\":\"standard\"}}"
  resolveConflicts: overwrite

managedNodeGroups:
- instanceType: t3.small
  amiFamily: AmazonLinux2
  name: album-app
  desiredCapacity: 2
  minSize: 2
  maxSize: 6
```

**Install ISTIO:** <https://ISTIO.io/latest/docs/setup/getting-started/#download>

**Install Cert Manager:** <https://cert-manager.io/docs/installation/>

**Install AWS Load Balancer Controller:** <https://kubernetes-sigs.github.io/aws-load-balancer-controller/v2.2/deploy/installation/>
