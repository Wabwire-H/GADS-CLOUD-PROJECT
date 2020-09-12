# GADS-CLOUD-PROJECT
## Lab1: **Getting Started with GKE**

### Objectives

	-Provision a Kubernetes cluster using Kubernetes Engine.

	-Deploy and manage Docker containers using kubectl.

### Steps

1. Set your Cloud Platform project in this session use gcloud config set project [PROJECT_ID] eg
	**gcloud config set project qwiklabs-gcp-04-fd10cb096a09**

2. Confirm that needed APIs are enabled;Kubernetes Engine API and Container Registry API

	List the services the current project has enabled for consumption, run:
	**gcloud services list --enabled**
	If either API is missing, enable them with the command gcloud services enable SERVICE_NAME in this case
	**gcloud services enable Kubernetes Engine API**
	**gcloud services enable Container Registry API**

3. For convenience, place the zone that Qwiklabs assigned you to into an environment variable called MY_ZONE
	**export MY_ZONE=us-central1-a**


4. Start a Kubernetes cluster managed by Kubernetes Engine. 
	Name the cluster webfrontend and configure it to run 2 nodes:
	**gcloud container clusters create webfrontend --zone $MY_ZONE --num-nodes 2**

5. After the cluster is created, check your installed version of Kubernetes
	**kubectl version**

6. View your running nodes
	**gcloud container node-pools list --cluster=webfrontend --zone $MY_ZONE**

### Run and deploy a container

1. launch a single instance of the nginx container
	**kubectl create deploy nginx --image=nginx:1.17.10**

2. View the pod running the nginx container
	**kubectl get pods**

3. Expose the nginx container to the Internet
	**kubectl expose deployment nginx --port 80 --type LoadBalancer**

4. View the new service:
	**kubectl get services**

Results a tabulated view eg

 NAME      |    TYPE       |    CLUSTER-IP  |    EXTERNAL-IP  |  PORT(S)    |      AGE
-----------|---------------|----------------|-----------------|-------------|-----------
kubernetes |   ClusterIP   |   10.51.240.1  |   <none>        |     443/TCP |       5m13s
nginx      |   LoadBalancer |  10.51.242.233 |  35.239.12.110 |    80:31981/TCP |   2m53s

5. Open a new web browser tab and paste your cluster's external IP address into the address bar.

Results: The default home page of the Nginx browser is displayed.

6. Scale up the number of pods running on your service:
	**kubectl scale deployment nginx --replicas 3**

7. Confirm that Kubernetes has updated the number of pods:
	**kubectl get pods**

8. Confirm that your external IP address has not changed:
	**kubectl get services**

Results: A tabulated view similar to the one above with an exact external IP address


9. Return to the web browser tab in which you viewed your cluster's external IP address. Refresh the page to confirm that the nginx web server is still responding.

Results: The default home page of the Nginx browser is displayed.



## Lab2: **Console and Cloud Shell**
### Objectives
In this lab, you learn how to perform the following tasks:

    -Get access to Google Cloud.

    -Create a Cloud Storage bucket using the Cloud Console.

    -Create a Cloud Storage bucket using Cloud Shell.

*Become familiar with Cloud Shell features.*
1. Create a bucket using the Cloud Console
	**gsutil mb gs://bark_at**
Results: creates a bucket called 'bark_at'

2. Create a second bucket and verify in the Cloud shell
	**gsutil mb gs://bark_at2**
	Listing created buckets
	**gsutil ls**
Results: The second bucket should be displayed in the Buckets list.

4. Explore more Cloud Shell features
   Upload any file from your local machine to the Cloud Shell VM
	**gsutil cp C:\Users\User\Documents\gadsproject.txt gs://bark_at/**
Results: Uploads a txt file named gads project to the bucket 'bark_at'

5. Confirm that the file was uploaded
	**ls**

*Create a persistent state in Cloud Shell*
1. To list available regions, execute the following command:
	**gcloud compute regions list**
2. Select a region from the list and note the value in any text editor.
	Type in the numeric value that corresponds to your region.

*This region will now be referred to as [YOUR_REGION] in the remainder of the lab.*
1. Create an environment variable and replace [YOUR_REGION] with the region you selected in the previous step: eg
	**INFRACLASS_REGION=us-west1-a**
2. Verify it with echo
	**echo $INFRACLASS_REGION**

*Append the environment variable to a file*
1. Create a subdirectory for materials used in this class
	**mkdir infraclass**
2. Create a file called config in the infraclass directory
	**touch infraclass/config**
3. Append the value of your Region environment variable to the config file:
	**echo INFRACLASS_REGION=$INFRACLASS_REGION >> ~/infraclass/config**
4. Create a second environment variable for your Project ID,replacing [YOUR_PROJECT_ID] with your Project ID.
	**INFRACLASS_PROJECT_ID=qwiklabs-gcp-04-fd10cb096a09**
5. Append the value of your Project ID environment variable to the config file:
	**echo INFRACLASS_PROJECT_ID=$INFRACLASS_PROJECT_ID >> ~/infraclass/config**
6. Use the source command to set the environment variables, and use the echo command to verify that the project variable was set:
	**source infraclass/config**
	**echo $INFRACLASS_PROJECT_ID**
7. Close and re-open Cloud Shell. Then issue the echo command again:
	**exit**
*Reopen cloud shell by clicking* **>_**

**echo $INFRACLASS_PROJECT_ID**
Results:There will be no output because the environment variable no longer exists.

*Modify the bash profile and create persistence*
1. Edit the shell profile with the following command:
	**nano .profile**
2. Add the following line to the end of the file:
	**source infraclass/config**
3. Press Ctrl+O, ENTER to save the file, and then press Ctrl+X to exit nano.
4. Close and then re-open Cloud Shell to cycle the VM.
5. Use the echo command to verify that the variable is still set:
	**echo $INFRACLASS_PROJECT_ID**

Results: You should now see the expected value that you set in the config file.