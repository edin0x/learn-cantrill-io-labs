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

# Stage 1: Creating and configuring an S3 bucket
Now that we have an AWS account, we need to configure the necessary resources for hosting a static site. Recall that we want to use S3 and CloudFront to host our static site. We'll start by creating an S3 bucket to store our static files.

Here's how to do it:
1. In the search bar at the top of the page, search for "S3" and select the S3 service.
3. Click the "*Create Bucket*" button to create a new S3 bucket.
4. For *AWS Region*, select `eu-central-1`.
5. Leave the rest of the settings as default for now and click the "Create" button to create the bucket.

Great! You've just created an S3 bucket where we can store our static files.

# Stage 2: Creating and configuring a CloudFront distribution
Now that we have an S3 bucket, we need to configure a CloudFront distribution to serve our static files from S3. 

<!-- TODO -->

Now you have an AWS account and the necessary resources for hosting a static site. In the next stage, we'll set up an Amazon CodeCommit repository to store the source code for our static site.

# Stage 3: Storing the source code with AWS
Before we can start building and deploying our static site, we need to store the source code for our site in a version control system. In this tutorial, we'll use AWS CodeCommit to store the source code for our site.

<!-- TODO -->
