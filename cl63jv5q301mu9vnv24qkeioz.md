## Creating a Kubernetes Cluster on AWS with Amazon EKS (Elastic Kubernetes Service)

Kubernetes is an open source container orchestration engine for automating deployment, scaling, and management of containerized applications. Kubernetes enables us to easily ship new code and effectively scale our application. A cluster is created when Kubernetes is deployed. <br>
Amazon Elastic Kubernetes Service (Amazon EKS) is a managed Kubernetes service that makes it easy for you to run Kubernetes on AWS and on-premises. 
Amazon EKS provisions and scales the Kubernetes control plane, including the application programming interface (API) servers and backend persistence layer, across multiple AWS Availability Zones (AZs) for high availability and fault tolerance. 
<br>
You can use the AWS Management Console, AWS CLI or the eksctl command-line tool to get up and running with Amazon EKS in minutes. In this short and simple article, we will be creating an EKS cluster using the AWS console. 

### Create an EKS Cluster and Node Group with AWS Console
#### Step 1: Create IAM Roles for the EKS Cluster and Node Group 
***Create EKS Cluster IAM role:***
1. Proceed to the Roles tab in the [Identity and Access Management (IAM)]( https://console.aws.amazon.com/iam/) dashboard in the AWS Console.
2. Select ***Create role***
3. Choose the type of trusted entity:
<ul>
<li> Select **EKS** as the use case
<li> Choose **EKS-Cluster**
<li> Click Next: **Permissions** </ul>
![ *Selecting EKS as Use Case for MyEKSClusterRole*](https://cdn.hashnode.com/res/hashnode/image/upload/v1658664042568/r9nh15YBq.png align="center")
4. Click **Next: Tags** <br>
5. Click **Next: Review**
<ul>
<li> Give the role a name, e.g. *MyEKSClusterRole* 
</ul>
6. Click ***Create role*** <br>
 *A notification stating The role AWSServiceRoleForAmazonEKS has been created should appear.* 

<br>
***Create EKS Cluster Node Group:***
1. In the IAM **Roles** tab, select **Create role**.
2. Choose the type of trusted entity:
<ul>
<li> Choose **EC2** as the use case
<li> Select **EC2**
<li> Click **Next: Permissions**
</ul>
3. In the Attach permissions policies, search for each of the following and check the box to the left of the policy to attach it to the role:
<ul>
<li>*AmazonEC2ContainerRegistryReadOnly*
<li>*AmazonEKSWorkerNodePolicy*
<li> *AmazonEKS_CNI_Policy*
</ul>
![Attaching permissions policies](https://cdn.hashnode.com/res/hashnode/image/upload/v1658664548427/gR9dBLIQw.png align="left")
4. Click **Next: Tags**
5. Click **Next: Review**
<ul>
<li> Give the role a name, e.g. *MyNodeRole*
</ul>
6. Click ** Create role** <br>
*A notification stating The role AWSServiceRoleForAmazonEKSNodegroup has been created should appear.* 

![IAM Roles for EKSClusterRole & NodeGroup](https://cdn.hashnode.com/res/hashnode/image/upload/v1658664723154/x8DeZ-MfE.png align="left")
<br>


#### Step 2: Create an SSH Pair
1. Navigate to  the Amazon EC2 console at https://console.aws.amazon.com/ec2/.
2. In the navigation pane, under Network & Security, select **Key pairs** .
3. Select ***Create Key pair***:
  <ul>
    <li> Give the key pair a name, (e.g. *mykeypair*)
    <li> Select **RSA** for **Key pair type** 
    <li> For Private key file format, select **.pem** (to save the private key in a format that can be used with OpenSSH.)
  </ul>
4. Select ** Create key pair**
![Keypair creation](https://cdn.hashnode.com/res/hashnode/image/upload/v1658665350990/lSLaBlwu6.png align="left")

 <br>

#### Step 3: Create an EKS Cluster
1. Proceed to the **Clusters** tab in  [**Amazon EKS**](https://console.aws.amazon.com/eks/home#/clusters) dashboard in the AWS Console.
2. Select ***Create cluster*** from the *Add Cluster* button on the **EKS** dashboard.
3. Specify:
<ul>
<li> a unique ** Name** (e.g. *MyEKSCluster*)
<li> **Kubernetes Version** (e.g. *1.22* )
<li> **Cluster Service Role** (select the role you created above, e.g. *MyEKSClusterRole* ) </ul>
4. Click **Next**
5. In the **Specify networking** section, look for **Cluster endpoint access**, click the *Public* radio button (*to allow the cluster endpoint to be accessible from outside of your VPC*)
6. Click **Next** and **Next**
7. In **Review and create**, click ***Create*** <br>
The EKS cluster takes 5-15 minutes for to be created. 

![EKS Cluster Creation](https://cdn.hashnode.com/res/hashnode/image/upload/v1658666915179/PdUqOMvcj.png align="left")

>Troubleshooting: You might get a notification that says: "Cannot create cluster the targeted availability zone does not currently have sufficient capacity to support the cluster", if you do, choose another availability zone and try again. You can set the availability zone in the upper right corner of your AWS console, where you can also find your AWS Region eg *N.Virginia(us-east-1)*. <br>
To learn more about AWS Regions and Availability Zones, please visit their [Global Infrastructure.](https://aws.amazon.com/about-aws/global-infrastructure/regions_az/) 

<br>

#### Step 4: Create a Node Group
1. Select on the **Compute** tab in your newly-created cluster
2. Click **Add Node Group**
3. Specify:
<ul>
<li>a unique **Name** (e.g. *MyNodeGroup* )
<li>**Cluster Service Role** (select the role you created above, e.g. *MyNodeRole* )
</ul>
![Add Node Group](https://cdn.hashnode.com/res/hashnode/image/upload/v1658667043933/hgh5Ellbr.png align="left")
4. In **Node Group compute configuration**, set *instance type* to *t3.micro* and *disk size* to **4** to minimize costs
5. In **Node Group scaling configuration**, set the number of nodes to 2
6. Select **Next**
7. In **Node Group network configuration**, toggle on **Configure SSH access to nodes**
<ul>
<li>Select the EC2 pair created above (e.g. *mykeypair* )
<li>At the ***Allow SSH remote access from*** section, Select **All**
<li>Click **Next**
</ul>
8. Review the configuration and click *"Create"* 


![Screenshot 2022-07-24 at 1.04.44 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1658668084871/YpeU5h24o.png align="left")

> Congratulations you have created your first Kubernetes Cluster & NodeGroup on Amazon EKS!!

<br>

#### Step  5: Delete Running Services
<blockquote>
<p>
 IMPORTANT! Don't forget to delete the services you created when you no longer need them.
</p>
</blockquote>
 The services you created will need to be deleted in order. Each step can take several minutes.
<ul>
<li> You first delete the **Node Group**
<li> Then you delete the **EKS Cluster**
</ul>
<br>
**Additional Information:**
You can read more on creating an EKS Cluster on the official AWS EKS website [here](https://aws.amazon.com/eks/)