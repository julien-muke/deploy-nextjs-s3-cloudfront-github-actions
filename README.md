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

For me, i've built my portfolio website with Next.js for handling the user interface, Three.js for rendering 3D elements, Framer motion for beautiful animations, and styled with TailwindCSS.

