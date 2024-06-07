# ![aws](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/01cd6124-8014-4baa-a5fe-bd227844d263)     Deploy NextJS App to S3 and CloudFront with Github Actions.


## <a name="introduction">🤖 Introduction</a>

In this demo, we will explore a solution that leverages Amazon S3 and CloudFront, combined with the power of GitHub Actions, to simplify the deployment process and ensure a seamless experience for both developers and end-users.


## <a name="design">📐 Project Architecture</a>

![Blank diagram-15](https://github.com/julien-muke/deploy-nextjs-s3-cloudfront-github-actions/assets/110755734/050c9219-ec93-44a4-8b74-8a4aa1881314)


## <a name="steps">☑️ Steps</a>

* Create a DynamoDB table to store the items
* Build a Lambda function to handle the CRUD operations on the DynamoDB table
* Use S3 to store and host the web application's static files (HTML, CSS, and JavaScript)
* Create a CloudFront distribution to serve the S3-hosted static files with low latency


## ➡️ Step 1 - Create an Amazon S3 Bucket