FROM :  
https://github.com/GoogleCloudPlatform/cloud-builders-community/tree/master/docker-compose/examples/hello-world

COMMAND :
```bash
cd gcp_cicd_test/example/hello-world
gcloud builds submit --config=cloudbuild.yaml .
```