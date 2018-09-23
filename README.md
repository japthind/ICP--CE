# ICP--CE
Deploy private cloud using ICP
Step 1: Install Docker for your boot node only
Step 2: Set up the installation environment
        Log in to the boot node as a user with root permissions.
        Pull the IBM Cloud Private-CE installer image from Docker Hub, run the following command:
           sudo docker pull ibmcom/icp-inception:3.1.0

     Create an installation directory to store the IBM Cloud Private configuration files in and change to that directory. For                example,to store the configuration files in /opt/ibm-cloud-private-ce-3.1.0, run the following commands:
           mkdir /opt/ibm-cloud-private-ce-3.1.0; 
           cd /opt/ibm-cloud-private-ce-3.1.0

    Extract the configuration files.
            sudo docker run -e LICENSE=accept -v "$(pwd)":/data ibmcom/icp-inception:3.1.0 cp -r cluster /data

    (Optional) You can view the license file for IBM Cloud Private by running the following command:
            docker run -e LICENSE=view -e LANG=$LANG ibmcom/icp-inception:3.1.0
    Where $LANG is a supported language format. For example, to view the license in Simplified Chinese, run the following command:
                docker run -e LICENSE=view -e LANG=zh_CN ibmcom/icp-inception:3.1.0

   Create a secure connection from the boot node to all other nodes in your cluster. Complete one of the following processes:
   Set up SSH in your cluster. See Sharing SSH keys among cluster nodes.
   Set up password authentication in your cluster. See Configuring password authentication for cluster nodes
        
   Add the IP address of each node in the cluster to the /<installation_directory>/cluster/hosts file
        
    If you use SSH keys to secure your cluster, in the /<installation_directory>/cluster folder, replace the ssh_key file with the            private key file that is used to communicate with the other cluster nodes. See Sharing SSH keys among cluster nodes. Run this            command:
                 sudo cp ~/.ssh/id_rsa ./cluster/ssh_key
                In this example, ~/.ssh/id_rsa is the location and name of the private key file.
                
Step 3: Customize your cluster
Ensure that the docker.io/ibmcom/* registry is in the allowed list of repository from which applications can be deployed. Add the following code to the config.yaml file:
     image-security-enforcement:
      clusterImagePolicy:
     - name: "docker.io/ibmcom/*"
       policy:
  Set up resource limits for proxy nodes. 
 You can also set a variety of optional cluster customizations that are available in the               /<installation_directory>/cluster/config.yaml file
 
 Step 4: Setup Docker for your cluster nodes as done for the boot node.
 
 Step 5: Deploy the environment
            Change to the cluster folder in your installation directory:
                  cd ./cluster

         Deploy your environment. Depending on your options, you might need to add more parameters to the deployment command.

  By default, the command to deploy your environment is set to deploy 15 nodes at a time. If your cluster has more than 15 nodes, the     deployment might take a longer time to finish. If you want to speed up the deployment, you can specify a higher number of nodes to be   deployed at a time. Use the argument -f <number of nodes to deploy> with the command.

To deploy your environment, run the following command:
sudo docker run --net=host -t -e LICENSE=accept -v "$(pwd)":/installer/cluster ibmcom/icp-inception:3.1.0 install

Verify the status of your installation.
If the installation succeeded, the access information for your cluster is displayed:

UI URL is https://master_ip:8443 , default username/password is admin/admin
In this message, master_ip is the IP address of the master node for your IBM Cloud Private-CE cluster.

Note: If you created your cluster within a private network, use the public IP address of the master node to access the cluster.

USE THE IP ADDRESS TO ACCESS THE ICP CONTROL PANEL 

END
