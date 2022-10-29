# gcp_cicd_test
GCP CI/CD test using Cloud Build

# Introduction
This project is for testing the Cloud Build using docker-compose command (+remote-builder)

# References
1. [GoogleCloudPlatform/cloud-builders-community](https://github.com/GoogleCloudPlatform/cloud-builders-community)
2. [Google Cloud Build를 사용한 CI/CD](https://zzsza.github.io/gcp/2020/05/09/google-cloud-build/)
3. [Cloud Build Remote Build Step](https://github.com/GoogleCloudPlatform/cloud-builders-community/tree/master/remote-builder)
4. [Permission error in GCP when creating a new compute instance but service account does have permissions](https://stackoverflow.com/questions/72701163/permission-error-in-gcp-when-creating-a-new-compute-instance-but-service-account)
5. [인스턴스 생성 및 구성](https://cloud.google.com/container-optimized-os/docs/how-to/create-configure-instance)
6. [Is it viable to run multiple containers on Google Compute Engine VM Instances running Container Optimized OS?](https://serverfault.com/questions/1032347/is-it-viable-to-run-multiple-containers-on-google-compute-engine-vm-instances-ru)
7. [Simple Installer for Docker Compose on Container-Optimized OS on Google Computing Engine](https://gist.github.com/kurokobo/25e41503eb060fee8d8bec1dd859eff3)
8. 

# Steps

### Step 1. Environment Setting

1. Create a Google [Google Cloud project](https://cloud.google.com/resource-manager/docs/creating-managing-projects?hl=ko)
   1. Recommend to use free trial mode
2. Install [Google Cloud SDK](https://cloud.google.com/sdk/docs/install?hl=ko)
   1. `./google-cloud-sdk/install.sh`
      1. install python using install.sh
      2. add ./google-cloud-sdk/bin direcotry to the `$PATH`
   2. `./google-cloud-sdk/bin/gcloud init`
      1. select the project which you created on the above step 1
3. Enable [Cloud Build API](https://console.cloud.google.com/flows/enableapi?apiid=cloudbuild.googleapis.com&redirect=https%3A%2F%2Fcloud.google.com%2Fcloud-build%2Fdocs%2Fquickstart-build&hl=ko&_ga=2.211218646.273633348.1588992492-151274966.1565535538)

### Step 2. Build [Cloud build](https://cloud.google.com/build/docs/cloud-builders?hl=ko)
1. Clone the `cloud-builders-community` repo:

   ```sh
   $ git clone https://github.com/GoogleCloudPlatform/cloud-builders-community
   ```

2. Go to the directory that has the source code for the `docker-compose` Docker image:

   ```sh
   $ cd cloud-builders-community/docker-compose
   ```

3. Build the Docker image:

   ```
   $ gcloud builds submit --config cloudbuild.yaml .
   ```

4. View the image in Google Container Registry:

   ```sh
   $ gcloud container images list --filter docker-compose
   ```

### Step 3. Test hello-world example
1. Clone the `gcp_cicd_test` repo:

   ```sh
   $ git clone https://github.com/kkkHoon/gcp_cicd_test.git
   ```

2. Go to the directory that has the source code for the hello-world:
```
$ cd gcp_cicd_test/example/hello-world
```

3. Build the Docker image:

   ```
   $ gcloud builds submit --config cloudbuild.yaml .
   ```
   
4. Check whether build is success or not:

   ```
   $ gcloud builds list
   ```
   
### Step 4. Add a new Trigger
1. Go to the [Cloud Build Trigger](https://console.cloud.google.com/cloud-build/triggers)
2. Click the `트리거 만들기(make trigger)` button
3. follow the guide line

### Step 5. Test your Trigger

### Done!

# +) Deploy to the Compute Engine
1. Enable "Compute instance administrator(v1) (Compute 인스턴스 관리자(v1))" on the Cloud Build > Setting
2. Go to the directory that has the source code for the `remote-builder` Docker image:

   ```sh
   $ cd cloud-builders-community/remote-builder
   ```
   
3. build the 'remote-builder':

   ```sh
   $ gcloud builds submit --config=cloudbuild.yaml .
   ```
   
4. Check 'remote-builder' is build successfully

   ```sh
   $ gcloud container images list --filter remote-builder
   ```
   
5. Create an appropriate IAM role with permissions to create and destroy Compute Engine instances in this project:

   ```sh
   $ export PROJECT=$(gcloud info --format='value(config.project)')
   $ export PROJECT_NUMBER=$(gcloud projects describe $PROJECT --format 'value(projectNumber)')
   $ export CB_SA_EMAIL=$PROJECT_NUMBER@cloudbuild.gserviceaccount.com
   $ gcloud services enable cloudbuild.googleapis.com
   $ gcloud services enable compute.googleapis.com
   $ gcloud projects add-iam-policy-binding $PROJECT --member=serviceAccount:$CB_SA_EMAIL --role='roles/iam.serviceAccountUser' --role='roles/compute.instanceAdmin.v1' --role='roles/iam.serviceAccountActor' --role='roles/compute.serviceAgent'
   ```
6. Test your Trigger..