# ddbctl-dtp-helm-operator

DynamoDB Delete Table Partition Data Helm Operator

**Clone ddbctl-dtp-operator git repository:**

```bash
$ cd ~\workspace\git.ws
$ git clone https://github.com/jittakal/ddbctl-dtp-helm-operator
$ cd ddbctl-dtp-helm-operator
```
**Deploy DdbctlDtpJob CRD:**

```bash
make deploy
```

Verify the helm operator deployment

```bash
$ kubectl get crd # new entry for ddbctldtpjob.dbctl.dtp.charts.operators.jittakal.io/v1alpha1

$ kubectl get namespaces # new entry for ddbctl-dtp-helm-operator-system

$ kubectl get pods -n ddbctl-dtp-helm-operator-system

$ kubctl logs -f <<pod-name-from-above-command>> -n ddbctl-dtp-helm-operator-system
```

## How it Works

The `ddbctl-dtp-heml-operator` extends the functionality of Kubernetes by introducing a custom resource, DdbctlDtpJob based on Helm Charts. This resource allows you to express your intent to delete a specific partition in a DynamoDB table directly through Kubernetes manifests.

```yaml
apiVersion: ddbctl.dtp.charts.operators.jittakal.io/v1alpha1
kind: DdbctlDtpJob
metadata:
  name: ddbctldtpjob-sample
spec:
  ddbCtlDtpJob:
    awsRegion: us-east-1
    endpointURL: http://aws-dynamhttp://dynamodb.local:8000
    partitionValue: partition-key-value
    tableName: my-dynamodb-table
```

- Customer Resource for DdbctlDtpJob CRD

```bash
$ kubectl create -f delete_partition_data_tablename_job.yaml

$ # Sample
$ kubectl create -f config/samples/ddbctl.dtp.charts_v1alpha1_ddbctldtpjob.yaml
```

- View Delete Table Partition Data Job - Summary

```bash
$ kubectl get jobs # default - namespace

$ kubectl get pods

$ kubectl logs <<podname-of-delete-table-partition-data-job>> 

$ # verify log table name and number of records delete 
$ # delete summary report on job completion
```

## Reference

- [DynamoDB Delete Parition Data Library](https://github.com/jittakal/dynamo-partition-delete)
