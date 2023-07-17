# Deploying a static website to AWS

This project includes a simple CloudFormation template that can be used to set up a static website on AWS (perfect for hosting React apps!).

Deploying the template will create:
1. An S3 bucket to store the static website files.
2. A CloudFront distribution that sits in front of S3.

The S3 bucket is configured to use Origin Access Control (OAC) which means the S3 bucket is NOT public and will only allow traffic to be served through CloudFront.

The CloudFront distribution will also override 404 requests to serve the `/index.html` which enables front-end routing.

### One click deployment

Use this **[One Click Deployment link](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=static-website&templateURL=https://s3.amazonaws.com/tadmatic-templates/aws-static-website/aws-static-website.yaml)** to open the AWS CloudFormation console.

### Manually deploy the CloudFormation template

Alternatively you can do the following:

1. Login to AWS console
2. Navigate to CloudFormation and click `Create Stack`
3. Click on `Upload a template` and upload `aws-static-website.yaml`
4. Enter a name for the stack and the name of the S3 bucket.
5. Click Next until you reach the end of the wizard.

Once the stack has been deployed you should see outputs:
1. S3 Bucket URI
2. CloudFront Distribution ID
3. CloudFront Website URL

### Deploying a React app to AWS

From your React project folder, run the following from the command line (assuming you have AWS CLI set up):
```
rm -rf ./build
yarn build # or npm run build
aws s3 sync ./build/ s3://MY_S3_BUCKET_NAME
aws cloudfront create-invalidation --distribution-id MY_CLOUDFRONT_DISTRIBUTION_ID --paths "/index.html"
```

This will do the following:
1. Build your React app into a `build` folder.
2. Sync the build folder with S3 (i.e. upload the files to S3)
3. Invalidate the CloudFront cache for the main `index.html` file

Note: replace MY_S3_BUCKET_NAME and MY_CLOUDFRONT_DISTRIBUTION_ID with outputs from the CloudFormation stack.
