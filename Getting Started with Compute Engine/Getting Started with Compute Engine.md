# Lab: Google Cloud Fundamentals: Getting Started with Compute Engine

## Overview:

In this lab, you will create virtual machines (VMs) and connect to them. You will also create connections between the instances.
             
## Objectives: 

In this lab, you will learn how to perform the following tasks:
- Create two Compute Engine virtual machines using the gcloud command-line interface.
- Connect between the two instances.

## Steps: 
1 Create a Compute Engine Virtual Machine (VM) using the gcloud command-line interface using the following command.

```
gcloud compute instances create my-vm-1 --zone=us-central1-a --machine-type=n1-standard-1 \
--subnet=default --tags=http-server --image=debian-9-stretch-v20200910 --image-project=debian-cloud
```

2 Create a `firewall-rule` that allows HTTP traffic into the `my-vm-1` instance via port 80 using the following command.

```$xslt
gcloud compute firewall-rules create default-allow-http --direction=INGRESS \--priority=1000 \
--network=default --action=ALLOW --rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http-server
```

3 To display a list of all the zones in a region, use the following command:

```$xslt
gcloud compute zones list | grep us-central1
```

4 Choose a zone from the result of the above command and set your default zone to it.

```$xslt
gcloud config set compute/zone us-central1-b
```

5 Create the second Virtual Machine (VM) using the gcloud command line with the following command

```$xslt
gcloud compute instances create "my-vm-2" --machine-type "n1-standard-1" --image-project "debian-cloud" \
--image "debian-9-stretch-v20190213" --subnet "default"
```

## Connect the two VM instances
## Steps:
1 Run the following command to ssh into `my-vm-2`
```$xslt
gcloud compute ssh  my-vm-2
```
input `Y` when prompted

2 Use the ping command to confirm that `my-vm-2` can reach `my-vm-1` over the network:
```$xslt
ping my-vm-1
```
Notice that the output of the ping command reveals that the complete hostname of `my-vm-1` is `my-vm-1.c.PROJECT_ID.internal`, where PROJECT_ID is the name of your Google Cloud Platform project. GCP automatically supplies Domain Name Service (DNS) resolution for the internal IP addresses of VM instances.

3 Press `Ctrl+C` to abort the ping command.

4 Use the ssh command to open a command prompt on `my-vm-1`:
```$xslt
ssh my-vm-1
```
If you are prompted about whether you want to continue connecting to a host with unknown authenticity, enter yes to confirm that you do.

5 At the command prompt on `my-vm-1`, install the Nginx web server:
```$xslt
sudo apt-get install nginx-light -y
```

6 Use the nano text editor to add a custom message to the home page of the web server:
```$xslt
sudo nano /var/www/html/index.nginx-debian.html
```
7 Use the arrow keys to move the cursor to the line just below the h1 header. Add text like this, and replace YOUR_NAME with your name:
```$xslt
Hi from YOUR_NAME
```

8 Press `Ctrl+O` and then press Enter to save your edited file, and then press Ctrl+X to exit the nano text editor.

9 Confirm that the web server is serving your new page. At the command prompt on `my-vm-1`, execute this command:
```$xslt
curl http://localhost/
```
The response will be the HTML source of the web server's home page, including your line of custom text.

10 To exit the command prompt on `my-vm-1`, execute this command:
```$xslt
exit
```

11 To confirm that `my-vm-2` can reach the web server on `my-vm-1`, at the command prompt on `my-vm-2`, execute this command:
```$xslt
curl http://my-vm-1/
```
The response will again be the HTML source of the web server's home page, including your line of custom text.

12 To get the IP address for `my-vm-1` run the following command:
```$xslt
gcloud compute instances list --zone=us-central1-a
```
Copy the IP address of `my-vm-1` and paste into the address bar of a browser tab. You will see your web server's home page, including your custom text.