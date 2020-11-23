cd4ml: https://martinfowler.com/articles/cd4ml.html

# Minio
Minio: https://raw.githubusercontent.com/minio/minio/master/docs/orchestration/docker-compose/docker-compose.yaml
`mkdir -p buckets/mlflow`
to install client, brew install gcc, brew install minio/stable/mc
https://nexiality.github.io/documentation/userguide/InstallingMinio.html
```
mc alias set s3 http://localhost:9000
mc ls s3/mlflow
```
sketchy script
```
def pluginParameter="junit pipeline-maven generic-webhook-trigger"
def plugins = pluginParameter.split()
println(plugins)
def instance = Jenkins.getInstance()
def pm = instance.getPluginManager()
def uc = instance.getUpdateCenter()
def installed = false

plugins.each {
  if (!pm.getPlugin(it)) {
    def plugin = uc.getPlugin(it)
    if (plugin) {
      println("Installing " + it)
      plugin.deploy()
      installed = true
    }
  }
}

instance.save()
if (installed)
```
mc admin config set s3 notify_webhook:1 queue_limit="0" queue_dir='.' endpoint="http://34.106.127.142/generic-webhook-trigger/invoke"
mc admin service restart s3
mc event add s3/mlflow/ arn:minio:sqs::1:webhook --suffix .jpg




Mlflow: https://towardsdatascience.com/deploy-mlflow-with-docker-compose-8059f16b6039

Sql: ommited volumes as a hacky workaround for this error: https://github.com/docker-library/mysql/issues/275

docker-compose --env-file ./.env up

1. exec into container and run the tests.py thing

## argo https://argoproj.github.io/argo-cd/getting_started/

1. `kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml`
2. `kubectl port-forward -n argocd svc/argocd-server 8080:80`

By default user is "admin" and pasword is the name of the server pod.

Install argocd for mac `brew install argocd`
1. `argocd login localhost:8080`
2. Enter admin and server password

## seldon https://docs.seldon.io/projects/seldon-core/en/v1.1.0/workflow/quickstart.html
## https://docs.seldon.io/projects/seldon-core/en/latest/examples/seldon_core_setup.html#Setup-Cluster
1. first create namespace `kubectl create ns seldon-system
2. install `helm install seldon-core seldon-core-operator --repo https://storage.googleapis.com/seldon-charts --set ambassador.enabled=true --set usageMetrics.enabled=true --namespace seldon-system`
3. install ambassador...
4. kubectl port-forward $(kubectl get pods -n seldon-system -l app.kubernetes.io/name=ambassador -o jsonpath='{.items[0].metadata.name}') -n seldon-system 8003:8080

## Jenkins
Follow steps here: https://www.jenkins.io/doc/book/installing/docker/
actually fuck that do this:
https://itnext.io/docker-inside-docker-for-jenkins-d906b7b5f527
docker-compose up
e789302744bc444ca77c25cb3ad5394d
