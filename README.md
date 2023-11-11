# ddbctl-dtp-helm-operator

DynamoDB Delete Table Partition Data Helm Operator

## Pre-requisite

```bash
$ brew install operator-sdk
```

## Steps

Steps followed to create Helm Operator using operator-framework

- Clone empty Github Repository

```bash
$ cd ~/workspace/git.ws
$ git clone https://github.com/jittakal/ddbctl-dtp-helm-operator.git
```

- Helm Operator - Init (Used existing Helm Chart)

```bash
$ cd ddbctl-dtp-helm-operator
$ operator-sdk init --plugins helm.sdk.operatorframework.io/v1 \
    --domain operators.jittakal.io \
    --group ddbctl.dtp.charts \
    --helm-chart ~/Workspaces/git.ws/dynamo-partition-delete/helm/ddbctl-dtp-job/
```

- Modify the Makerfile

```yaml
IMAGE_TAG_BASE ?= docker.io/jittakal/ddbctl-dtp-helm-operator
IMG ?= $(IMAGE_TAG_BASE):$(VERSION)
```

- Build Operator docker image and publish to docker.io repository

```bash
$ make docker-build docker-push
```

- Run Operator - locally

```bash
$ make install run
```
- Run Operator - inside cluster

```bash
$ make deploy

# Verify the deployment

$ kubectl get namespaces # new namespace - ddbctl-dtp-helm-operator-system

$ kubectl get deployment -n ddbctl-dtp-helm-operator-system # deployment for controller manager

$ kubectl get pods -n ddbctl-dtp-helm-operator-system # pod for controller-manager

```

- Sample DynamoDB Delete Table Partition Data Job

```bash
$ kubectl apply -f config/samples/ddbctl.dtp.charts_v1alpha1_ddbctldtpjob.yaml

# Verify the Job deployment

$ kubectl get pods # Job pod should be running/completed
```
