# Kubernetes commands / tips

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
