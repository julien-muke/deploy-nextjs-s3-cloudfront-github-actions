# ![aws](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/01cd6124-8014-4baa-a5fe-bd227844d263)     Deploy NextJS App to S3 and CloudFront with Github Actions.


## <a name="introduction">🤖 Introduction</a>

In this demo, we will explore a solution that leverages Amazon S3 and CloudFront, combined with the power of GitHub Actions, to simplify the deployment process and ensure a seamless experience for both developers and end-users.


## <a name="design">📐 Project Architecture</a>

![Blank diagram-15](https://github.com/julien-muke/deploy-nextjs-s3-cloudfront-github-actions/assets/110755734/050c9219-ec93-44a4-8b74-8a4aa1881314)


## <a name="steps">☑️ Steps</a>

* Create a Create NextJS App
* Push Source Code to GitHub
* Create S3 Bucket
* Setting permissions for website access
* Upload the app to S3 Bucket
* Create CloudFront Distribution
* Integrate AWS with Github for Github Actions Workflow
* Set up IAM roles to connect GitHub Actions to actions in AWS
* Configuring OpenID Connect in Amazon Web Services


## ➡️ Step 1 - Create and configure a Next.js 13 app

To create a new Next.js application, Run this command:

```bash
    npx create-next-app@latest YOUR-PROJECT-NAME
```

Once you created the application, navigate to the project directory and open your project with a code editor (VS code). You can create more files within your application.

For me, i've a Modern UI/UX website using React.js & Tailwind CSS called KivuBank.

Note: If you want a website example, i have a few React.js apps in my ![GitHub repo](https://github.com/julien-muke) that you can clone into your local machine and use for this AWS demo (Feel free to follow and give it a star ⭐).


## ➡️ Step 2 - Push your Next.js 14 app to GitHub

When you're done building your Next.js application, You have to push your code to GitHub.
Create a GitHub repository and push your Next.js app's code to this repository by using the following command:


```bash
git init
git add .
git commit -m "Your commit message"
git branch -M master
git remote add origin <your_repository_url>
git push -u -f origin master
```

## ➡️ Step 3 - Create S3 Bucket

1. Sign in to the AWS Management Console and open the Amazon S3 console at https://console.aws.amazon.com/s3/.
2. In the left navigation pane, choose Buckets.
3. Choose Create bucket
4. Under Bucket type, choose General purpose.
5. For Bucket name, enter a name for your bucket. I will name it `nextjs-s3-cloudfront-jm`

![Create-S3-bucket-S3-us-east-1(1)](https://github.com/julien-muke/deploy-nextjs-s3-cloudfront-github-actions/assets/110755734/b3a1de0d-e8fd-4715-833a-f86dc5d23269)

6. Clear Block all public access, leave the rest as default and choose Save changes.

Public access is granted to buckets and objects through access control lists (ACLs), bucket policies, access point policies, or all. In order to ensure that public access to this bucket and its objects is blocked, turn on Block all public access.

![Create-S3-bucket-S3-us-east-1(1) copy](https://github.com/julien-muke/deploy-nextjs-s3-cloudfront-github-actions/assets/110755734/60bcf88c-d314-4191-948f-be40ed5db8fd)

7. Next, let's navigate to Tab, choose "properties", scroll down to "static website hosting", click "Edit"

![Screenshot 2024-06-08 at 14 51 20](https://github.com/julien-muke/deploy-nextjs-s3-cloudfront-github-actions/assets/110755734/da5a51bb-b653-4b3b-9efc-96661e84114c)


8. To edit static website hosting:
    <br>* "Enable" Static website hosting
    <br>* Choose "Host a static website" as hosting type
    <br>* Specify the home or default page of the website as `index.html`
    <br>* Error document as `404.html`

![Edit-static-website-hosting-S3-bucket-nextjs-s3-cloudfront-jm-S3-us-east-1](https://github.com/julien-muke/deploy-nextjs-s3-cloudfront-github-actions/assets/110755734/a87058a0-9d4a-4377-83b2-791b188df6cb)


## ➡️ Step 4 - Setting permissions for website access

1. Go to your S3 Bucket console, on the Tab section, choose "permissions"
2. Scroll down to "Bucket policy" and click "Edit"
3. Copy and paste the Bucket policy below
4. Click "Save changes"

Note: Make sure to change the Bucket ARN and paste it to the under your resources.


```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::Bucket-Name/*"
            ]
        }
    ]
}
```


![Edit-bucket-policy-S3-bucket-nextjs-s3-cloudfront-jm-S3-us-east-1](https://github.com/julien-muke/deploy-nextjs-s3-cloudfront-github-actions/assets/110755734/f87e5baf-836c-4bb2-9a22-e7b5f90bfb66)


## ➡️ Step 5 - Upload the app to S3 Bucket

1. Back to S3 Bucket console, click on the S3 Bucket we've created
2. Click "Upload"
3. Open your Nextjs App on VS Code, extract your code by running the following command"

```bash
npm run build
```

4. This command will extract your code into "dist folder" then copy and paste in S3 Bucket
5. Then click "Upload"

![4](https://github.com/julien-muke/deploy-nextjs-s3-cloudfront-github-actions/assets/110755734/eadae769-6ca4-4e5f-93de-67ecd2ce8e3b)

6. To check our website, let's go back to S3 Bucket properties and scroll down to "static website hosting" and click on the link provided, you will see our app hosted.

![Screenshot 2024-06-08 at 14 53 01](https://github.com/julien-muke/deploy-nextjs-s3-cloudfront-github-actions/assets/110755734/b76df35a-1055-47a6-81c3-0d63286b557c)

![4](https://github.com/julien-muke/deploy-nextjs-s3-cloudfront-github-actions/assets/110755734/a90f6c9c-d627-4787-bb21-86bafaf980f7)


## ➡️ Step 6 - Create CloudFront Distribution

To create a distribution (console):

1. Sign in to the AWS Management Console and open the CloudFront console at https://console.aws.amazon.com/cloudfront/v4/home.
2. In the navigation pane, choose Distributions, then choose Create distribution.

![Screenshot 2024-06-10 at 15 03 29](https://github.com/julien-muke/deploy-nextjs-s3-cloudfront-github-actions/assets/110755734/5b39b6be-703a-4503-a4f3-28e95b025bea)

3. Under Origin domain, choose our S3 Bucket
4. To use this distribution as a website, choose S3 website endpoint
5. Under Protocol, choose HTTP only
6. Enter a name for this origin, which is our S3 Bucket

![CloudFront-Global](https://github.com/julien-muke/deploy-nextjs-s3-cloudfront-github-actions/assets/110755734/9ef62f3e-e69c-4415-b837-11416df292dd)

7. For Default cache behavior, under Viewer protocol policy choose Redirected HTTP to HTTPS

![CloudFront-Global copy](https://github.com/julien-muke/deploy-nextjs-s3-cloudfront-github-actions/assets/110755734/04cbb27a-38d9-4123-a77f-67c68c355d52)


8. For Web Application Firewall (WAF), choose Do not enable security protection
9. Under Settings, for default root object enter `index.html`
10. Leave everything as default then click "Create distrition"


![CloudFront-Global copy 2](https://github.com/julien-muke/deploy-nextjs-s3-cloudfront-github-actions/assets/110755734/a965c1f6-7150-4604-8d33-cd0f5773f409)


To check if the cloudfront distribution has been successfully deployed, copy and paste the Distribution domain name on a new tab

![Screenshot 2024-06-13 at 12 10 39](https://github.com/julien-muke/deploy-nextjs-s3-cloudfront-github-actions/assets/110755734/724c3fb9-234b-46ac-b03f-6b8b5debadfa)

![Screenshot 2024-06-13 at 12 12 29](https://github.com/julien-muke/deploy-nextjs-s3-cloudfront-github-actions/assets/110755734/2a60956e-a243-44fa-8aba-86ed27f70f95)


## ➡️ Step 7 - Integrate AWS with Github for Github Actions Workflow

GitHub Actions is a continuous integration and continuous delivery (CI/CD) platform that allows you to automate your build, test, and deployment pipeline. You can create workflows that run tests whenever you push a change to your repository, or that deploy merged pull requests to production.

Prerequisites:

  <br>* You have a repository on GitHub.com where you can add files.
  <br>* You have access to GitHub Actions.

To create your first workflow:

1. In your repository on GitHub.com, create a workflow file called `github-actions-demo.yml` in the `.github/workflows` directory. 

![Screenshot 2024-06-16 at 10 54 07](https://github.com/julien-muke/deploy-nextjs-s3-cloudfront-github-actions/assets/110755734/379414d8-5cfe-4faf-a5ab-6957720eaecb)


2. Copy the following YAML contents into the `github-actions-demo.yml` file:

```yml
---
  name: Build and Deploy NextJS App to S3 and CloudFront
  on:
    push:
      branches: [main]
  # These permissions are needed to interact with GitHub's OIDC Token endpoint.
  permissions:
    id-token: write
    contents: read
  jobs:
    build-and-deploy:
      name: Build and Deploy
      runs-on: ubuntu-latest
  
      env:
        NEXTJS_DIST: dist
        AWS_REGION: us-east-1
        S3_BUCKET: YOUR-S3-BUCKET-NAME
        CLOUDFRONT_DISTRIBUTION_ID: YOUR-CLOUDFRONT-DISTRIBUTION-ID
  
      steps:
        - name: Checkout
          uses: actions/checkout@v3
  
        - name: Configure AWS credentials from AWS account
          uses: aws-actions/configure-aws-credentials@v3
          with:
            role-to-assume: arn:aws:YOUR-ARN-ROLE-NAME/github-to-aws-oicd
            aws-region: ${{ env.AWS_REGION }}
  
        - name: Install Dependencies
          run: |
            node --version
            npm ci --production
            npm install @types/react 
            npm install @types/node
  
        - name: Build Static Website
          run: npm run build
  
        - name: Copy files to the production website with the AWS CLI
          run: |
            aws s3 sync --delete ${{ env.NEXTJS_DIST }} s3://${{ env.S3_BUCKET }}
  
        - name: Copy files to the production website with the AWS CLI
          run: |
            aws cloudfront create-invalidation \
              --distribution-id ${{ env.CLOUDFRONT_DISTRIBUTION_ID }} \
              --paths "/*"
```

NOTE: Make sure yu update `YOUR-S3-BUCKET-NAME` `YOUR-CLOUDFRONT-DISTRIBUTION-ID` `arn:aws:YOUR-ARN-ROLE-NAME/github-to-aws-oicd`

3. Click Commit changes, committing the workflow file to a branch in your repository triggers the push event and runs your workflow.
Before we test the workflow, let's create an IAM Policy to attach to the S3 Bucket.


## ➡️ Step 8 - Create an  IAM Policy to attach to S3 Bucket

To create a policy:

1. Sign in to the AWS Management Console and open the IAM console at https://console.aws.amazon.com/iam/.
2. In the navigation pane on the left, choose Policies. 
3. Choose Create policy.

![Screenshot 2024-06-16 at 15 22 16](https://github.com/julien-muke/deploy-nextjs-s3-cloudfront-github-actions/assets/110755734/669c0485-dd83-4804-81af-38bcf417d6af)

4. Add permissions by selecting services, actions, resources, and conditions. Build permission statements using the JSON editor.

![Screenshot 2024-06-16 at 15 23 13](https://github.com/julien-muke/deploy-nextjs-s3-cloudfront-github-actions/assets/110755734/3dd1cc67-b6a2-4503-a79e-7c8a61194a9d)

5. Copy and paste the following policies into your JSON editor:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ListObjectsInBucket",
            "Effect": "Allow",
            "Action": "s3:ListBucket",
            "Resource": "arn:aws:s3:::Bucket-Name"
        },
        {
            "Sid": "AllObejctActions",
            "Effect": "Allow",
            "Action": "s3:*Object",
            "Resource": "arn:aws:s3:::Bucket-Name/*"
        },
        {
            "Sid": "InvalidateCF",
            "Effect": "Allow",
            "Action": "cloudfront:CreateInvalidation",
            "Resource": "*"
        }
    ]
}
```
NOTE: Make sure the bucket ARN name with yours

![Create-policy-IAM-Global(1)](https://github.com/julien-muke/deploy-nextjs-s3-cloudfront-github-actions/assets/110755734/525374c6-45c5-4db7-834c-535bce348a4a)


6. Click Next, under create and review, enter the policy name `s3-web-access-policy` and click "Create policy"

As you can see below the Permissions defined (cloudfront and S3) are already been attached in this policy.

![Create-policy-IAM-Global(2)](https://github.com/julien-muke/deploy-nextjs-s3-cloudfront-github-actions/assets/110755734/aa3cdeab-5ea7-4093-9d92-2ee4201e01b3)


## ➡️ Step 9 - Configuring OpenID Connect in Amazon Web 

OpenID Connect (OIDC) allows your GitHub Actions workflows to access resources in Amazon Web Services (AWS), without needing to store the AWS credentials as long-lived GitHub secrets.

To add the GitHub OIDC provider to IAM, we will Configuring the role and trust policy.

1. Back to IAM console, navigate to "role" click "create role"

![Screenshot 2024-06-16 at 15 28 51](https://github.com/julien-muke/deploy-nextjs-s3-cloudfront-github-actions/assets/110755734/345aaa7b-172e-422c-be19-25324ebfd41c)

2. Under Trusted entity type, choose "AWS service"
3. For Use case, choose "S3" click "Next"

![Create-role-IAM-Global(12)](https://github.com/julien-muke/deploy-nextjs-s3-cloudfront-github-actions/assets/110755734/1de70816-bccb-48ab-a445-664bd6478714)


4. Choose a policy to attach to your new role Permissions policy, we are going to choose `s3-web-access-policy` click "Next"

![Screenshot 2024-06-16 at 15 33 52](https://github.com/julien-muke/deploy-nextjs-s3-cloudfront-github-actions/assets/110755734/d2a851f3-09a9-4931-9c2d-cbd89a7561dc)

5. Enter role name `github-to-aws-oidc`
6. Click "Create role"

![Create-role-IAM-Global(13)](https://github.com/julien-muke/deploy-nextjs-s3-cloudfront-github-actions/assets/110755734/2af48a50-6911-4a4c-af38-5b9b16be75eb)

6. We are going to update our policy trusted entities, under Trust relationships Tab, choose "Edit trust policy"
7. Copy and paste the following policy entities:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Federated": "arn:aws:YOUR-ROLE-NAME/token.actions.githubusercontent.com"
            },
            "Action": "sts:AssumeRoleWithWebIdentity",
            "Condition": {
                "StringLike": {
                    "token.actions.githubusercontent.com:sub": "repo:YOUR-REPO-ACCOUNT-NAME/YOUR-SPECIFIC-REPO-NAME:*"
                },
                "ForAllValues:StringEquals": {
                    "token.actions.githubusercontent.com:iss": "https://token.actions.githubusercontent.com",
                    "token.actions.githubusercontent.com:aud": "sts.amazonaws.com"
                }
               
            }
        }
    ]
}
```

NOTE: Make sure you update your ARN Role Name and your GitHub repository.

![Screenshot 2024-06-16 at 15 52 31](https://github.com/julien-muke/deploy-nextjs-s3-cloudfront-github-actions/assets/110755734/9b26302e-4d0c-4b94-965f-fde09914c9ab)


## ➡️ Step 10 - Run and test the GitHub Actions Workflow

To viewing your workflow results:

1. On GitHub.com, navigate to the main page of the repository.
2. Under your repository name, click Actions.

![2](https://github.com/julien-muke/deploy-nextjs-s3-cloudfront-github-actions/assets/110755734/8cff4a4b-7271-4671-b40b-10fdf3093727)

3. In the left sidebar, click the workflow you want to display, in my case is `Build and Deploy NextJS App to S3 and CloudFront`

![Screenshot 2024-06-16 at 17 25 54](https://github.com/julien-muke/deploy-nextjs-s3-cloudfront-github-actions/assets/110755734/5f0a727a-579d-4ecf-b215-13573d56e9db)


4. The log shows you how each of the steps was processed successfully. Expand any of the steps to view its details.

![github_actions](https://github.com/julien-muke/deploy-nextjs-s3-cloudfront-github-actions/assets/110755734/aa14428f-bf3b-4022-bc7c-eb276923ea3d)

## Let's test the cloudfront distribution

1. Copy the Distribution domain name

![Screenshot 2024-06-13 at 12 10 39](https://github.com/julien-muke/deploy-nextjs-s3-cloudfront-github-actions/assets/110755734/eab77fb2-428d-4a23-a07b-91b71afda546)

2. As you can see below our Cloudfront distribution is working

![cloudfront](https://github.com/julien-muke/deploy-nextjs-s3-cloudfront-github-actions/assets/110755734/6a612007-25f7-4bb4-912c-4cba9a79d402)

To test the CI/CD Pipeline with GitHub Workflow, when you edit something in the code files and push the changes to GitHub, it will trigger each time the code is pushed to the GitHub branch, then go through S3 bucket and automatically updates cloudfront distribution.











