
# Step-by-step guide on setting up SSH, creating a key pair in AWS and using the key pair in Ubuntu to connect EC2 via SSH for Ansible 

## Setting up SSH
1. Open up a terminal and install OpenSSH server with the following command:
```
sudo apt-get install openssh-server
```
2. Verify that SSH is running by using:
```
sudo service ssh status
```

## Creating a Keypair in AWS
1. Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/.
2. In the navigation pane, under NETWORK & SECURITY, choose Key Pairs.
3. Choose Create key pair.
4. For Name, enter the name that you want to use for the key pair, and choose Create Key Pair.

## Using the Keypair in Ubuntu to connect to EC2
1. After the key pair is created, the private key is automatically downloaded by your browser. The base file name is the name you specified as the name of your key pair, and the filename extension is `.pem`. Save the private key file in `.ssh` directory.
2. Change the permission of the key pair file to read only for your user 
```
chmod 400 /path/my-key-pair.pem
```
3. To connect to your instance, use the ssh command.
```
ssh -i /path/my-key-pair.pem my-instance-user-name@my-instance-public-dns-name
```

## Use the keypair in Ansible
1. Install ansible with the following command:
```
sudo apt-get install ansible
```
2. Add the private key to ssh-agent using ssh-add (so that you don't have to use the -i flag with ansible command):
```
ssh-add /path/my-key-pair.pem
```
3. Now, create a hosts file in /etc/ansible (or any directory where you'll run ansible commands from), and add your EC2 instances:
```
[mygroup]
my-instance-public-dns-name
```
4. Test ansible ping:
```
ansible -m ping mygroup
```

## Note
In the ssh command and ansible hosts file, replace `/path/my-key-pair.pem` with the path where you saved your private key, and replace `my-instance-public-dns-name` with the public DNS or public IP of your EC2 instance.

Also, the `my-instance-user-name` will be dependent on the AMI that you have launched on your EC2 instance. For example, it is 'ubuntu' for Ubuntu AMIs and 'ec2-user' for most AWS and Centos AMIs.

These steps should get you started with setting up SSH, creating a keypair in AWS, and using the keypair in Ubuntu to connect to EC2 via SSH for use with Ansible.
