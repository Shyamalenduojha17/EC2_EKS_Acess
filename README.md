   # [Deploying Applications on an AWS EKS Cluster](https://github.com/Shyamalenduojha17/DemoBlazerApp-with-EKS): [Step-by-Step Setup Using EC2 Instance](https://github.com/Shyamalenduojha17/EC2_EKS_Acess/blob/master/README.md)

## Prerequities:
   - Create IAM role
   - Create EC2 Instance with t3.medium Machine type
   - Attach created IAM role With AWS EC2 instance
   - Insatall Kubectl
   - Install Eksctl
   - Install awscli
   - Create EKS Cluster using eksctl commands
   - Config IAM OIDC Provider
### 1. Create IAM role:
>  Open the **IAM console** at https://console.aws.amazon.com/iam/.

> In the left navigation pane, choose **Roles**.

> On the Roles page, choose **Create role**.


> On the **Select trusted entity** page, do the following: <br>
     a. In the **Trusted entity type** section, choose **AWS Service** <br>
     b. For **Service or Use case**, choose **EC2**.  (because we are going to acces EKS through EC2)  <br>
     c. Choose** Next** <br>

     
> On the **Add permissions** page, do the following: <br>
     a. On the Filter **policies** box, enter **AdministratorAccess**  <br>
     b. Select the check box to the left of the **AdministratorAccess** returned in the search.  <br>
     c. Choose **Next.**  <br>

> On the **Name, review, and create** page, do the following: <br>
     a. For **Role** name, enter a unique name for your role, such as **AmazonEKS_Access_Role** <br>
     b. Under Add tags (Optional), add metadata to the role by attaching tags as key–value pairs. <br>
     c. Choose Create role. <br>

### 2. Create EC2 Instane with t3.medium machine type:

> Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/.

> From the EC2 console dashboard, in the Launch instance box, choose Launch instance, and then choose Launch instance from the options that appear.

> Under Name and tags, for Name, enter a descriptive name for your instance.

> Under Application and OS Images (Amazon Machine Image), do the following:  <br>
  a. Choose Quick Start, and then choose Ubuntu 22.04 LTS. This is the operating system (OS) for your instance. <br>

> Under Instance type, from the Instance type list, you can select the hardware configuration for your instance. <br>
  a. Choose the t2.micro instance type. <br>

> Under Key pair (login), for Key pair name, choose the key pair that you created when getting set up.

> Click on Launch instance

### 3. Attach created IAM role With AWS EC2 instance:

> In Instance info, Select the check box to the left of the running instance which was created before. <br>

> In right top side, Go to action. Click on it. <br>

> In bottom side of Action Menu, click on Security > click on Modify IAM role.  <br>

> In search bar, search your IAM role and select it.  <br>

> Click on attach role.  <br>

Then connect with EC2 Instance and Install following tools: <br>

### 4. Insatall Kubectl:
 
Kubectl is a command line tool that you use to communicate with the Kubernetes API server.

> For Linux
```
curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.28.1/2023-09-14/bin/linux/amd64/kubectl
chmod +x ./kubectl
mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$HOME/bin:$PATH
kubectl version --client

```

### 5. Install Eksctl:

eksctl is a simple command line tool for creating and managing Kubernetes clusters on Amazon EKS. eksctl provides the fastest and easiest way to create a new cluster with nodes for Amazon EKS. For the official documentation, see https://eksctl.io/

> For Linux
```
# for ARM systems, set ARCH to: `arm64`, `armv6` or `armv7`
ARCH=amd64
PLATFORM=$(uname -s)_$ARCH

curl -sLO "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_$PLATFORM.tar.gz"
# (Optional) Verify checksum
curl -sL "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_checksums.txt" | grep $PLATFORM | sha256sum --check
tar -xzf eksctl_$PLATFORM.tar.gz -C /tmp && rm eksctl_$PLATFORM.tar.gz
sudo mv /tmp/eksctl /usr/local/bin
eksctl version

```

### 6. Install awscli:
The AWS Command Line Interface (AWS CLI) is an open source tool that enables you to interact with AWS services using commands in your command-line shell. 

> For Linux:

```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version

```

### 7. Create EKS Cluster using eksctl commands:

```
eksctl create cluster --name eksdemo --region us-west-1 --instance-selector-vcpus=2 --instance-selector-memory=4
```
For more customization details look at https://eksctl.io/getting-started/  and  https://eksctl.io/usage/creating-and-managing-clusters/ <br>

## Afrer EKS Cluster is Created
When first installing kubectl, it isn't yet configured to communicate with any server. If you ever need to update the configuration to communicate with a particular cluster, you can run the following command.
```
aws eks update-kubeconfig --region region-code --name my-cluster
```
Replace **_region-code_** with the AWS Region that your cluster is in. Replace ** _my-cluster_** with the name of your cluster. <br>

### 8. Config IAM OIDC Provider:

```
eksctl utils associate-iam-oidc-provider --name eksdemo --region us-west-1 --approve
```

