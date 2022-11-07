# Kubernetes amazon-cloudwatch-logs

[AWS for Fluent Bit](https://github.com/aws/aws-for-fluent-bit) is a [Fluent Bit](https://github.com/fluent/fluent-bit) docker image customized to work best with AWS EKS, AWS Cloudwatch, etc.

## Installing the Chart

Before you can install the chart you will need to add the `ironhalik.github.io/charts/` repo to [Helm](https://helm.sh/).

```shell
helm repo add ironhalik https://ironhalik.github.io/charts/
```

After you've installed the repo you can install the chart.

```shell
helm upgrade --install amazon-cloudwatch-logs ironhalik/amazon-cloudwatch-logs
```

## Configuration

The following table lists the configurable parameters of the _amazon-cloudwatch-logs_ chart and their default values.

| Parameter                            | Description                                                                                                                                          | Default                                               |
| ------------------------------------ | -----------------------------------------------------------------------------------------------------------------------------------------------------| ----------------------------------------------------- |
| `awsRegion`                          | The region where this chart is deployed. Will be used for log creation, AWS API communication.                                                       | `""`                                                  |
| `clusterName`                        | Name of the cluster the chart is deployed to. By default, will be used for log group creation.                                                       | `""`                                                  |
| `logs.groupBaseName`                 | The base name of AWS log groups that will be created and used.                                                                                       | `/aws/eks/${CLUSTER_NAME}`                            |
| `logs.retentionDays`                 | Log retention in days                                                                                                                                | `""` (Never expire)                                   |
| `logs.readFromHead`                  | https://docs.fluentbit.io/manual/pipeline/inputs/tail#config                                                                                         | `false`                                               |
| `logs.readFromTail`                  | https://docs.fluentbit.io/manual/pipeline/inputs/systemd#configuration-parameters                                                                    | `true`                                                |
| `image.repository`                   | Image repository.                                                                                                                                    | `public.ecr.aws/aws-observability/aws-for-fluent-bit` |
| `image.tag`                          | Image tag, will override the default tag derived from the chart app version.                                                                         | `""`                                                  |
| `image.pullPolicy`                   | Image pull policy.                                                                                                                                   | `IfNotPresent`                                        |
| `image.pullSecrets`                  | Image pull secrets.                                                                                                                                  | `[]`                                                  |
| `nameOverride`                       | Override the `name` of the chart.                                                                                                                    | `nil`                                                 |
| `fullnameOverride`                   | Override the `fullname` of the chart.                                                                                                                | `nil`                                                 |
| `metrics.enabled`                    | If `true`, allow unauthenticated access to `/metrics`.                                                                                               | `false`                                               |
| `metrics.port`                       | On which port metrics should listen                                                                                                                  | `2020`                                                |
| `serviceAccount.create`              | If `true`, create a new service account.                                                                                                             | `true`                                                |
| `serviceAccount.annotations`         | Annotations to add to the service account.                                                                                                           | `{}`                                                  |
| `serviceAccount.name`                | Service account to be used. If not set and `serviceAccount.create` is `true`, a name is generated using the full name template.                      | `nil`                                                 |
| `serviceAccount.secrets`             | The list of secrets mountable by this service account. See https://kubernetes.io/docs/reference/labels-annotations-taints/#enforce-mountable-secrets | `[]`                                                  |
| `rbac.create`                        | If `true`, create the RBAC resources.                                                                                                                | `true`                                                |
| `commonLabels`                       | Labels to add to each object of the chart.                                                                                                           | `{}`                                                  |
| `hostNetwork.enabled`                | If `true`, start the pod in hostNetwork mode.                                                                                                        | `true`                                                |
| `resources`                          | Resource requests and limits for the _metrics-server_ container. See https://github.com/kubernetes-sigs/metrics-server#scaling                       | `{"limits":{"cpu":0.5,"memory":"400Mi"},"requests":{"cpu":0.05,"memory":"150Mi"}}`                                                  |
| `tolerations`                        | Tolerations for pod assignment.                                                                                                                      | `[{"effect":"NoSchedule","key":"node-role.kubernetes.io/master","operator":"Exists"},{"effect":"NoExecute","operator":"Exists"},{"effect":"NoSchedule","operator":"Exists"}]`                                                  |

## Huge thank you to:
[Metrics Server Chart](https://github.com/kubernetes-sigs/metrics-server/tree/master/charts/metrics-server) from which I lifted general code guidelines for this chart (and a lot of code :))  
[aws-samples kubernetes manifests](https://github.com/aws-samples/amazon-cloudwatch-container-insights) on which this chart is based