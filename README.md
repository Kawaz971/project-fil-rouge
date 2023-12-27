# project-fil-rouge

# Création de 4 instance sur AWS + Sécurity Group + Key Pair (Mettre dans les sous réseaux adaptés)
- 1 X Ubuntu server T2.small for Jenkins-Server (IP PUBLIC)
- 1 X Ubuntu Server t2.micro for Docker-server
- 2 X Ubuntu server té.medium for Kubernetes-server-AZ1 & Kubernetes-server-AZ1

# Installation du Jenkins-server (Connexion via la key pair + IP public) 
sudo apt install openjdk-11-jdk-headless -y
sudo apt update -y
sudo apt upgrade -y

curl -fsSL https://pkg.jenkins.io/debian/jenkins.io.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins -y

sudo systemctl status jenkins
sudo systemctl start jenkins
sudo systemctl enable --now jenkins

# Premiere connexion server Jenkins avec i@ip!8080
sudo cat /var/lib/jenkins/secrets/initialAdminPassword

# Configuration Jenkins-server
Allez dans Manage Pluging > Installer SSH Agent

# Installation Docker-server
Etape à suivre
  # Add Docker's official GPG key:
    sudo apt-get update
    sudo apt-get install ca-certificates curl gnupg
    sudo install -m 0755 -d /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    sudo chmod a+r /etc/apt/keyrings/docker.gpg

  # Add the repository to Apt sources:
    echo \
     "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
     $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt-get update
  # Install docker
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
 
  # ERREUR FREQUENT = Got permission denied 
solution : sudo chmod 644 /var/run/docker.sock


# Installation K3S sur la machine Kubernetes-Server 172.20.4.155
curl -sfL https://get.k3s.io | sh -s - --write-kubeconfig-mode 644

Desinstallation = /usr/local/bin/k3s-uninstall.sh

# Installation de HELM sur la machine Kubernetes 172.20.4.155
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh

# Erreur error loading config file "/etc/rancher/k3s/k3s.yaml
sudo chmod 644 /etc/rancher/k3s/k3s.yaml

# Erreur lancement HELM (Error: INSTALLATION FAILED: Kubernetes cluster unreachable: Get "http://localhost:8080/version": dial tcp 127.0.0.1:8080: connect: connection refused)
Solution : export KUBECONFIG=/etc/rancher/k3s/k3s.yaml

# Changement de Namespace
sudo kubectl config set-context --current --namespace=filrouge-prod

# Suppression Realase Helm 
helm uninstall filrouge-chart-prod 

# Lister HELM namespace
helm list --all-namespaces

# Création redirection sur cloudns.net TYPE CNAME
filrouge-prod.kawaz.cloudns.ph > my-load-balancer-2054635803.eu-west-3.elb.amazonaws.com

# Création  du Load Balancer
Ajout des zone de disponiblités eu-west-3B & eu-west-3a (Sous réséaux publix)
Creation d'un groupe cible (sous réseaux privé )
Ajout du protocole 80 et 443