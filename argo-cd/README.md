

helm repo add argo https://argoproj.github.io/argo-helm
helm repo up

helm diff upgrade -n argo argo-cd . \
   --set gitRepo.sshPrivateKey="`cat ~/.ssh/id_rsa_tgc_argo`" \
   -f environment/shared.yaml

helm upgrade -i -n argo argo-cd . --create-namespace \
   --set gitRepo.sshPrivateKey="`cat /path/to/priv/key`" \
   -f environment/shared.yaml

helm template -n argo argo-cd . \
   -f environment/shared.yaml

helm upgrade -i -n argo argo-cd . --create-namespace \
   -f environment/shared.yaml