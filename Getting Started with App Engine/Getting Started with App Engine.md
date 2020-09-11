# Lab: Google Cloud Fundamentals: Getting Started with App Engine

## Overview:

In this lab, you create and deploy a simple App Engine application using a virtual environment in the Google Cloud Shell.
             
## Objectives: 

In this lab, you learn how to perform the following tasks:
- Initialize App Engine.
- Preview an App Engine application running locally in Cloud Shell.
- Deploy an App Engine application, so that others can reach it.
- Disable an App Engine application, when you no longer want it to be visible.

## Task 1: Initialize App Engine


### Steps: 
1 In GCP console, on the top right toolbar, click the Open Cloud Shell button and click continue.

2 Initialize your App Engine app with your project and choose its region:
```$xslt
gcloud app create --project=$DEVSHELL_PROJECT_ID
```
When prompted, select the region where you want your App Engine application located.

3 Clone the source code repository for a sample application in the `hello_world` directory:
```$xslt
git clone https://github.com/GoogleCloudPlatform/python-docs-samples
```

4 Navigate to the source directory:
```$xslt
cd python-docs-samples/appengine/standard_python3/hello_world
```

## Task 2: Run Hello World application locally

### Steps:
1 Execute the following command to download and update the packages list:
```$xslt
sudo apt-get update
```

2 Set up a virtual environment in which you will run your application. Python virtual environments are used to isolate package installations from the system.
```$xslt
sudo apt-get install virtualenv
```
If prompted [Y/n], press Y and then Enter 
```$xslt
virtualenv -p python3 venv
```

3 Activate the virtual environment.
```$xslt
source venv/bin/activate
```

4 Navigate to your project directory and install dependencies.
```$xslt
pip install  -r requirements.txt
```

5 Run the application:
```$xslt
python main.py
```

6 In Cloud Shell, click **Web preview () > Preview on port** `8080` to preview the application.

7 To end the test, return to Cloud Shell and press `Ctrl+C` to abort the deployed service.


## Task 3: Deploy and run Hello World on App Engine

To deploy your application to the App Engine Standard environment:

### Steps:
1 Navigate to the source directory:
```$xslt
cd ~/python-docs-samples/appengine/standard_python3/hello_world
```

2 Deploy your Hello World application.
```$xslt
gcloud app deploy
```
If prompted "Do you want to continue (Y/n)?", press Y and then Enter.
This `app deploy` command uses the `app.yaml` file to identify project configuration.

3 Launch your browser to view the app at `http://YOUR_PROJECT_ID.appspot.com`
```$xslt
gcloud app browse
```
Copy and paste the URL into a new browser window.