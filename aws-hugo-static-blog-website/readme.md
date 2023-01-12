# Building and deploying a static blog site using AWS and Hugo

## Overview
Welcome to the lab on building and deploying a static blog site using AWS and Hugo! In this lab, you'll walk through the steps involved in setting up an AWS account, configuring the necessary resources for hosting and building a static site, installing Hugo, creating a new static site, and deploying the site to the cloud. By the end of this lab, you'll have a fully-functional static blog site that's cheap, fast, secure, and scalable.

We'll be using S3 and CloudFront to host our static site. S3 is a highly scalable object storage service that allows us to store and serve static content. CloudFront is a content delivery network (CDN) service that allows us to serve our static content from a global network of edge locations.

We'll also be using Route 53 to configure a custom domain name for our site. Route 53 is a highly available and scalable Domain Name System (DNS) web service.

The hands-on part of this lab is divided into stages. Each stage builds on the previous stage, so you'll need to complete the previous stages before you can move on to the next stage.

Before we get started, let's take a brief look at static websites and static site generators.

## Brief introduction into static websites and static site generators
A static website is a type of website that is built with fixed content. The content is built using HTML, CSS, and JavaScript and is stored on a web server.

The content of a static website does not change unless the website owner manually updates the content of the website, which means that they can be served directly from a web server without the need for a backend system or database. This can provide several benefits:
- **Fast and scalable:** because static content is directly served without the need for server-side processing or database queries, static websites are fast, efficient and easier to scale
- **More secure:** because there is no risk of any backend and/or database vulnerabilities.
- **Cheaper:** because static websites do not require a backend system to serve content, they are generally speaking cheaper to host compared to dynamic websites.

While static websites offer many benefits, they also have some disadvantages:
- **Limited functionality:** Because static websites do not have a backend database or server-side processing, they are limited in terms of the types of functionality they can offer. For example, a static website cannot handle user input or provide personalized content to users.
- **Lack of flexibility:** It can be time-consuming and difficult to update the content of a static website, as it requires manual changes to the HTML, CSS, and JavaScript files. This can make it difficult to quickly respond to changing needs or make frequent updates to the website.

### Static site generators
To overcome some of these disadvantages, we can use a static site generator. A static site generator is a tool that generates a static website from source files. Some popular static site generators include [Jekyll](https://jekyllrb.com/), [Hugo](https://gohugo.io/), and [Gatsby](https://www.gatsbyjs.com/). Using a static site generator can provide several benefits, such as:
- **Ease of use:** Static site generators allow developers to focus on writing content rather than worrying about the underlying infrastructure of the website.
- **Version control:** Static site generators make it easy to track changes and revert to previous versions of the website using version control systems like Git.
- **Reusability:** Static site generators allow developers to reuse templates and design elements across multiple pages of a website, making it easier to maintain a consistent look and feel.
- **Scalability:** Static websites are easier to scale compared to dynamic websites because they do not have to handle server-side processing and database queries.
- **Performance:** As mentioned before, static websites are fast and efficient, which can improve the overall user experience of the website.
    
In this tutorial, we'll be using Hugo to build and deploy a static blog site to AWS. Hugo is a popular static site generator that is written in Go and is open-source. It's fast, flexible, and easy to use, which makes it a great choice for building and deploying a static blog site.

So let's get started! In the next sections, we'll show you how to set up an AWS account and configure the necessary resources for hosting a static site.

# Stage 0: Setting up an AWS account
Before we can start building and deploying our static site, we'll need to set up an AWS account and configure the necessary resources. It is fairly straightforward. Here's how to do it:

1. Go to [aws.amazon.com](https://aws.amazon.com) and click the "*Create an AWS account*" button.
2. Follow the prompts to create a new AWS account and enter your billing information.
3. Once your account is set up, log in to the [AWS Management Console](https://console.aws.amazon.com/).

Great! You've just set up an AWS account. The next step is to configure an IAM user for programmatic access. This will allow us to manage AWS resources from our local machine.

# Stage 1: Configuring an IAM user for programmatic access
In order to manage AWS resources from our local machine, we'll need to configure an IAM user for programmatic access. Here's how to do it:
1. In the search bar at the top of the page, search for "IAM" and select the IAM service.
2. In the left sidebar, click the "Users" link.
3. Click the "Add user" button.
4. Enter a name for the user and select the "Programmatic access" checkbox. We'll be using `hugo` as the reference user name for this tutorial.
5. Click the "Next: Permissions" button.
6. Click the "Attach existing policies directly" button.
7. Search for "AWSCodeCommitPowerUser" and select the checkbox next to it. This will allow the user to access AWS CodeCommit.
8. Click the "Next: Tags" button.
9. Click the "Next: Review" button.
10. Click the "Create user" button.
11. Make sure to save the Access key ID and Secret access key. You'll need these to configure AWS CLI on your local machine. You can also download the .csv file containing the credentials.

You've just configured an IAM user for programmatic access. The next step is to configure AWS CLI on your local machine.

# Stage 2: Configuring AWS CLI
AWS CLI is a command-line tool that allows you to manage AWS resources from your local machine. It's a great tool to have, so let's get it set up.

Here's how to do it:
1. Go to the [AWS CLI download page](https://aws.amazon.com/cli/) and follow the instructions to download and install AWS CLI on your local machine.
2. Once AWS CLI is installed, open a terminal and run the following command to configure AWS CLI:
    ```bash
    aws configure
    ```
3. Enter your AWS Access Key ID and AWS Secret Access Key. You can find these in the [AWS Management Console](https://console.aws.amazon.com/).
4. For the default region name, enter `eu-central-1` (or any other region of your choice)
5. For the default output format, enter `json`.

You've just set up AWS CLI. The next step is to configure the necessary resources for hosting a static site.

# Stage 3: Configuring Git and CodeCommit
Before we can start building and deploying our static site, we need to store the source code for our site in a version control system. In this stage, we'll use AWS CodeCommit to store the source code for our site remotely at AWS. The code stored in CodeCommit will be used to build and deploy our site to our earlier configured S3 bucket. For local development, we need to configure git. So let's setup git first.

On most Linux and Mac machines, git is already installed. If not, you can install it from [here](https://git-scm.com/downloads). Choose your operating system and follow the instructions to install git.

Now that we have git installed, we need to configure it to use CodeCommit. Here's how to do it:
1. Open a terminal and run the following command: `git config --global credential.helper '!aws codecommit credential-helper $@'`. This instructs git to use AWS CodeCommit as the credential helper to authenticate with CodeCommit using the AWS credentials that we obtained previously.
2. Now also run: `git config --global credential.UseHttpPath true`. This instructs git to supply the path portion of the remote URL to credential helpers.

1. In the search bar at the top of the page, search for "CodeCommit" and select the CodeCommit service.
2. Click the "Create repository" button to create a new repository.
3. For *Repository name*, enter a name for your repository. As an example reference, we'll use `static-blog-website` for the remainder of this tutorial.
4. Click the "Create" button to create the repository.
5. Click the "Clone URL" button to copy the HTTPS clone URL for the repository.
6. Open a terminal and navigate to the directory where you want to store the source code for your site.
7. In the terminal run the `git clone <copied-https-url>` command to clone the remote repository locally. For example, if the HTTPS clone URL is `https://git-codecommit.eu-central-1.amazonaws.com/v1/repos/static-blog-website`, then the command would be `git clone https://git-codecommit.eu-central-1.amazonaws.com/v1/repos/static-blog-website`. This will clone the repository to a directory called `static-blog-website` in the current directory.

Awesome! You've just configured git to use CodeCommit and cloned the remote repository locally. The next step is to create a Hugo site and push it to CodeCommit.

# Stage 4: Creating a Hugo site and pushing it to CodeCommit
<!-- TODO -->

# Stage 5: Creating and configuring an S3 bucket
Now that we have an AWS account, we need to configure the necessary resources for hosting a static site. Recall that we want to use S3 and CloudFront to host our static site. We'll start by creating an S3 bucket to store our static files.

Here's how to do it:
1. In the search bar at the top of the page, search for "S3" and select the S3 service.
3. Click the "*Create Bucket*" button to create a new S3 bucket.
4. For the bucket name choose a for you recognizable name. It helps if this is the web address to your website. The name must also be globally unique (in case it isn't an error message will be shown). As an example reference, we'll use  `www.static-blog-website.com` for the remainder of this tutorial as the name of the bucket and domain.
5. For *AWS Region*, select `eu-central-1`, or any other region that is close to you. This is the region where your bucket will be created.
6. Leave the rest of the settings as default for now and click the "Create" button to create the bucket.

Great! You've just created an S3 bucket where we can store our static files. The next step is to configure a CloudFront distribution to serve our static files from S3.

# Stage 6: Creating and configuring a CloudFront distribution
CloudFront is a content delivery network (CDN) service that allows you to securely deliver content to viewers with low latency and high transfer speeds. We'll use CloudFront to serve our static files from S3.

Here's how to do it:
1. In the search bar at the top of the page, search for "CloudFront" and select the CloudFront service.
2. Click the "Create Distribution" button to create a new CloudFront distribution.
3. For *Origin Domain Name*, select the S3 bucket that we created in the previous stage. As the example case, we'll select `www.static-blog-website.com.s3.eu-central-1.amazonaws.com`. The origin is the location where CloudFront gets the files that it will serve to viewers.
4. Leave the rest of the settings as default for now and click the "Create Distribution" button to create the distribution.
5. Wait for the distribution to be deployed. This may take a few minutes.

Now you have an AWS account and the necessary resources for hosting a static site. In the next stage, we'll set up an Amazon CodeCommit repository to store the source code for our static site.
