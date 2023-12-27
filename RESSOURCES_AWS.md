# Création du VPC
Name Tag : my-vpc
IP : 172.20.0.0/20

# CREATE Subnet
Selectionner le VPC

Subnet name : my-public-web-subnet-1
IP : 172.20.1.0/24
Availabilité Zone : eu-west-3a

Subnet name : my-public-web-subnet-2
IP : 172.20.2.0/24
Availabilité Zone : eu-west-3b

Subnet name : my-public-web-subnet-3
IP : 172.20.3.0/24
Availabilité Zone : eu-west-3c

Subnet name : my-public-app-subnet-1
IP : 172.20.4.0/24
Availabilité Zone : eu-west-3a

Subnet name : my-public-app-subnet-2
IP : 172.20.5.0/24
Availabilité Zone : eu-west-3b

Subnet name : my-public-web-subnet-3
IP : 172.20.6.0/24
Availabilité Zone : eu-west-3c

Subnet name : my-public-db-subnet-1
IP : 172.20.7.0/24
Availabilité Zone : eu-west-3a

Subnet name : my-public-db-subnet-2
IP : 172.20.8.0/24
Availabilité Zone : eu-west-3b

Subnet name : my-public-db-subnet-3
IP : 172.20.9.0/24
Availabilité Zone : eu-west-3c

# CREATE Route Table

name : my-public-web-route-table
Selectionner le VPC

name : my-private-app-route-table
Selectionner le VPC

name : my-private-db-route-table
Selectionner le VPC

# Route Table Subnet Association
my-public-web-route-table ==<> my-public-web-subnet-1 my-public-web-subnet-2 my-public-web-subnet-3
my-private-app-route-table ==<> my-private-app-subnet-1 my-private-app-subnet-2 my-private-app-subnet-3
my-private-db-route-table ==< my-private-db-subnet-1 my-private-db-subnet-2 my-private-db-subnet-3>

# Create Internet Gateway
Name : my-internet-gateway
Attach to VPC

# Create NAT Gateway
name : my-nat-gateway-1
Attribuer une Elastic IP

# Route Tables
Séléctionner : my-public-web-route-table
Allez dans route == edit route
add route destination 0.0.0.0/0 Target igw.xxxxxxx

Séléctionner : my-private-app-route-table
Allez dans route == edit route
add route destination 0.0.0.0/0 Target nat.xxxxxxx

Séléctionner : my-private-db-route-table
Allez dans route == edit route
add route destination 0.0.0.0/0 Target nat.xxxxxxx

