# gcp_cicd_test
GCP CI/CD test using Cloud Build

# Introduction
This project is for testing the Cloud Build using docker-compose command

# References
1. [GoogleCloudPlatform/cloud-builders-community](https://github.com/GoogleCloudPlatform/cloud-builders-community)
2. [Google Cloud Build를 사용한 CI/CD](https://zzsza.github.io/gcp/2020/05/09/google-cloud-build/)

# Steps

### Step 1. Environment Setting

1. Create a Google [Google Cloud project](https://cloud.google.com/resource-manager/docs/creating-managing-projects?hl=ko)
   1. Recommend to use free trial mode
2. Install [Google Cloud SDK](https://cloud.google.com/sdk/docs/install?hl=ko)
   1. `./google-cloud-sdk/install.sh`
      1. install python using install.sh
      2. add ./google-cloud-sdk/bin direcotry to the `$PATH`
   2. `./google-cloud-sdk/bin/cloud init`
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