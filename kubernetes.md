# Kubernetes commands / tips

<!-- TOC -->

- [Kubernetes commands / tips](#kubernetes-commands--tips)
    - [How to set a default namespace](#how-to-set-a-default-namespace)
    - [How to get logs using the app name](#how-to-get-logs-using-the-app-name)
    - [How to delete pods using the app name](#how-to-delete-pods-using-the-app-name)
    - [How to create deployment secret to access github repositories](#how-to-create-deployment-secret-to-access-github-repositories)

<!-- /TOC -->

## How to set a default namespace
```
kubectl config set-context --current --namespace=<NAMESPACE> 
```

## How to get logs using the app name

```bash
function get_log_by_app() {
   kubectl logs -f $(
      kubectl get pods -n "${1}" \
         --selector "app=${2}" \
         --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}' 
      )
}
get_log_by_app <NAMESPACE> <APP>
```

## How to delete pods using the app name

```bash
function delete_pods_by_app() {
   kubectl delete pod $(
      kubectl get pods -n "${1}" \
         --selector "app=${2}" \
         --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}' 
      )
}
delete_pods_by_app <NAMESPACE> <APP>
```

## How to create deployment secret to access github repositories

_https://github.com/kubernetes/git-sync/blob/master/docs/ssh.md_

1) Create the deployment key 
2) Set up SSH config file:
   ```
   Host airflow-dags-repo
   Hostname github.com
   IdentityFile /home/<user>/.ssh/id_rsa
   ```

3) Scan the ssh key and generate the known hosts file
   ```
   ssh-keyscan github.com > /tmp/known_hosts
   ```
4) Create the secret
  ```
  kubectl create secret generic <secret_name> \
      --from-file=known_hosts=/tmp/known_hosts \
      --from-file=gitSshKey=$HOME/.ssh/id_rsa
  ```
