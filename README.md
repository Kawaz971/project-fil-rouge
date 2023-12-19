# project-fil-rouge


# Installation K3S sur la machine Kubernetes 172.20.4.155
curl -sfL https://get.k3s.io | sh -s - --write-kubeconfig-mode 644

# Installation de HELM sur la machine Kubernetes 172.20.4.155
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh


# Erreur lancement HELM (Error: INSTALLATION FAILED: Kubernetes cluster unreachable: Get "http://localhost:8080/version": dial tcp 127.0.0.1:8080: connect: connection refused)
Solution : export KUBECONFIG=/etc/rancher/k3s/k3s.yaml

# Création redirection sur cloudns.net TYPE CNAME
filrouge-prod.kawaz.cloudns.ph > my-load-balancer-2054635803.eu-west-3.elb.amazonaws.com

# Création  du Load Balancer
Ajout des zone de disponiblités eu-west-3B & eu-west-3a (Sous réséaux publix)
Creation d'un groupe cible (sous réseaux privé )
Ajout du protocole 80 et 443