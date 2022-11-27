# Gitlab CI/CD pipeline

### IaaC solution to build, containerize and deploy. 

This is a pipeline for Gitlab´s React sample app, it builds and tests the app, then builds the Docker container and pushes the image to Gitlab private registry, runs more tests and finally, deploys it to AWS Elastic Beanstalk.

## Installation and Setup


Theres a few things to keep in mind before having the pipeline up and running.


#### AWS 

First of all you need to ceate a new EB application, select Docker platform and sample application. Once you have this covered you should have a new enviroment, a new EC2 instance and a new S3 Bucket.

Is important to remember this and terminate any unwanted resources once you´re done to prevent charges on your AWS account.

Go to EB > Environments > Mywebsite-env and click on **Upload and deploy** and select the `Dockerrun.aws.public.json` file inside of the **templates** folder.

If everything is good you should see the Welcome to nginx default webpage in your EB app by now.

Now we´ll handle the roles and permissions part, head to IAM service, add new user with the name you like and select **Access key - programatic access** and your credential type.

Attach the required policies or select a user group is up to you, copy the access key ID and the Secret access key you´ll need them later.

Finally, we´ve got to edit a couple variables in the `.gitlab-ci.yml` line 60 and 61: 
`APP_NAME` and `APP_ENV_NAME` you´ll find this information under Applications inside EB.

#### Gitlab


First of all create a environment, copy the URL address of your EB app and in Gitlab go to CI/CD > Jobs > Environments and create a environment named `Production` store your URL in the Eternal URL field.

Theres some variables you need to edit on your Gitlab repo for the pipeline to work correctly.

You can find variables under Settings > CI/CD.

You need to add `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` with the credentials you previously generated in AWS, you should mask these two, also specify your region in `AWS_DEFAULT_REGION`.
Finally you need to specify your S3 Bucket in a variable named `AWS_S3_BUCKET`.

If you take a look at the auth.json file, there´s a variable called `DEPLOY_TOKEN` this is necessary for AWS to log into our private Docker registry in Gitlab and retrieve the image.

So you need to head to Settings > Repository > Deploy tokens and create a new one. Name it as you like and select the read_repository and read_registry scopes. Copy the token and store it in a variable named `GITLAB_DEPLOY_TOKEN` the value should be `AWS:PASTED_TOKEN`, you should mask this one too.







