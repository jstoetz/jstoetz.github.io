---
layout: collection
title:  "Hosting a Website on AWS"
---

## Hosting Options

1. Hosting on Your Own Computer
2. AWS Route 53 + AWS S3
2. External Registrar + AWS Route 53 + AWS S3
3. External Registrar + AWS Route 53 + AWS S3 + AWS Certificate Manager + AWS CloudFront + GitHub Actions
4. Github Pages
5. Netlify

## Introduction

The instructions outlined below are for hosting a static website (ie, no server side code).  It assumes you will have the following file types in your repository:
* **HTML**
* **CSS**
* **JavaScript**

## Option 3: External Registrar + AWS Route 53 + AWS S3 + AWS Certificate Manager + AWS CloudFront + GitHub Actions

### Option 3 - Tools and Accounts
Make sure you have setup accounts for the tools below.  All AWS products will only require the setup of one root user login.  Github and any external domain purchase is separate.

1. Registered Domain through AWS Route 53 or An External Registrar (GoDaddy etc.)
2. AWS S3
3. AWS CloudFront
4. GitHub Account
5. AWS CodePipeline
6. AWS Certificate Manager

### Option 3 - Steps

1. **Signup and/or Login to AWS Console**
* If you don't already have one, sign-up for an [Amazon Web Services Account](https://aws.amazon.com/)
* Login to the [AWS Console](https://console.aws.amazon.com/)

2. **Signup and/or Login to GitHub**
* If you don't already have one, sign-up up for a [Github Account](https://github.com/join)
* **Github Desktop** - download the appropriate version of [GitHub Desktop](https://desktop.github.com/)
* Note that these steps assume a Project Site and not a User or Organization site on GitHub.  More information on the difference between the three [can be found on the Get Started Guide in GitHub Pages](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages#types-of-github-pages-sites)

3. **Create a New Repository in Github**
* Choose one of the following methods:
  * Login to [GitHub](https://github.com/new) and create a new public repository.
  * Create a new repository with [GitHub Desktop](https://docs.github.com/en/desktop/installing-and-configuring-github-desktop/overview/creating-your-first-repository-using-github-desktop).

4. **Add Folders and Files to Repository**
* Create the following folder in your repository:
  * /css
  * /js
  * /img
* Add the following files to the root of your repository:
  * **index.html**
  * **error.html**
  * You can use any type of HTML you want for your **index.html** file or copy the following which is a modified version of [this template](https://www.freecodecamp.org/news/basic-html5-template-boilerplate-code-example/)

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>HTML 5 Boilerplate</title>
    <link rel="stylesheet" href="css/style.css">
  </head>
  <body>
	<script src="js/index.js"></script>
  </body>
</html>
```
* Here is a boilerplate template for the **error.html** from [here](https://github.com/h5bp/html5-boilerplate/blob/main/src/404.html)

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>HTML 5 Boilerplate</title>
    <link rel="stylesheet" href="css/style.css">
  </head>
  <bod>
    <h1>Page Not Found</h1>
    <p>Sorry, but the page you were trying to view does not exist.</p>
  </body>
</html>
```

* You can also add ```index.js``` to your /js directory and ```styles.css``` to your /css directory.

* Your Repository should look something like this:

```
.
|-- /css
    |-- styles.css
|-- /img
|-- /js
    |-- index.js
|-- index.html
|-- error.html

```

* If your file structure looks totally different, don't worry about it.  We will change it in the future - just make sure that the **index.html** and **error.html** files are in the root directory.

5. **Register for a Domain Name**
* Purchase and setup your domain name through your preferred registrar and login to the DNS Console in a separate tab.
  * This example assumes the domain is registered with [GoDaddy](https://www.godaddy.com).
* Make note of your new domain which should be something like ```https://example-site.com```.

6. **Create a New S3 Bucket**
* Login to the [AWS S3 console](https://s3.console.aws.amazon.com/s3/)
* Follow [the AWS Docs Tutorial](https://docs.aws.amazon.com/AmazonS3/latest/userguide/HostingWebsiteOnS3Setup.html) on Configuring a static website on Amazon S3.
  * Make sure you [Add a bucket policy that makes your bucket content publicly available](https://docs.aws.amazon.com/AmazonS3/latest/userguide/HostingWebsiteOnS3Setup.html#:~:text=Step%204%3A-,Add%20a%20bucket%20policy%20that%20makes%20your%20bucket%20content%20publicly%20available,-After%20you%20edit)
  * Make sure that you [DISABLE your S3 bucket's block public access settings](https://docs.aws.amazon.com/AmazonS3/latest/userguide/configuring-block-public-access-bucket.html)
  * Make note of your S3 bucket's endpoint without the leading **http://**.
  * For our example, the **S3 bucket website endpoint** should be something like ```http://example-site.com.s3-website.us-east-2.amazonaws.com```.

7. **Upload Files to Your New S3 Bucket**
* Recall in Step 4, you created **index.html** and **error.html**.  Upload both files to your New S3 Bucket.
  * For now, just upload the **index.html** and **error.html** files and not anything else.  In a later step, we will connect the repository to our S3 bucket for a seamless workflow.
  * Follow the AWS S3 Docs [To upload folders and files to an S3 bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/upload-objects.html#:~:text=To%20upload%20folders%20and%20files%20to%20an%20S3%20bucket) using the S3 Console.
* Test that these files have been uploaded by going to your S3 bucket's endpoint.
  * For our example, we should enter ```http://example-site.com.s3-website.us-east-2.amazonaws.com``` in our browser's url bar and see our **index.html** file displayed.

8. **Create a CloudFront Distribution**
*  Create a CloudFront Distribution by following the [AWS Docs for Creating a CloudFront Distribution](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-creating-console.html)
  * For **Origin domain** enter the endpoint that you copied in step 6. Note: Don't select the bucket from the dropdown list. The dropdown list includes only the S3 Bucket REST API endpoints that aren't used in this configuration.
    * For our example, our **Origin domain** would be ```http://example-site.com.s3-website.us-east-2.amazonaws.com```
  * For **Viewer protocol policy** select **Redirect HTTP to HTTPS**
  * Under **Alternate domain name (CNAME)** add the following:
    * ```*.example-site.com``` so any future subdomains will be re-directed.
  * Under ***Custom SSL certificate*** select "Request Certificate".  Note: complete the steps below then return to the **Create Distribution** tab.
    * This will open a new tab to the [AWS Certificate Manager](https://console.aws.amazon.com/acm/home?region=us-east-1#/welcome) for your appropriate S3 Bucket's region.
      * Select **Request public certificate**
      * Under **Fully qualified domain name** enter:
        * ```*.example-site.com```
        * Use an asterisk (*) to request a wildcard certificate to protect several sites in the same domain. For example, *.example.com protects www.example.com, site.example.com, and images.example.com.
      * Under **Select validation method** choose **DNS validation**.
    * Once you have created the certificate select **View Certificate**
      * You will need the following values:
        * **CNAME Name**
          * This will look like ```_a79865eb4cd1a6ab990a45779b4e0b96.example-site.com.```
        * **CNAME Value**
          * This will look like ```_424c7224e9b0146f9a8808af955727d0.hkmpvcwbzw.acm-validations.aws.```
    * Login to your Registrar's DNS Console for the Domain purchased in Step 5.  For this example, I am using GoDaddy.
      * Go to **My Domains / Domain Settings / DNS Management / DNS Records**.
      * Select **Add**
        * Under **Type** select **CNAME**
        * Under **Name** copy and paste the CNAME Name from AWS Certificate Manager.  For our example, this will look like ```_a79865eb4cd1a6ab990a45779b4e0b96.example-site.com.```.  Note that since we are using the GoDaddy, you need to modify the Name by truncating the apex domain (including the period) at the end of the NAME field, as follows: ```_a79865eb4cd1a6ab990a45779b4e0b96```. For more information, see [DNS validation on GoDaddy fails](https://docs.aws.amazon.com/acm/latest/userguide/troubleshooting-DNS-validation.html#troubleshooting-DNS-GoDaddy).
        * Under **Value** copy and paste the CNAME value from AWS Certificate Manager.  For our example, this will look like ```_424c7224e9b0146f9a8808af955727d0.hkmpvcwbzw.acm-validations.aws.```
  * Back on the **AWS CloudFront - Create Distribution** tab, enter **index.html** for the **Default root object**.
  * Select **Create Distribution**
    * Note: you can only create the distribution once you have verified the SSL certificate with AWS Certificate Manager. This took less than 10 minutes for me, but can take longer depending on multiple different variables.

8. **Use AWS 53 to Route Traffic to the Amazon CloudFront distribution**
* Sign in to the AWS Management Console and open the [Route 53 console](https://console.aws.amazon.com/route53/)
* Setup a Hosted Zone for the website hosted on GoDaddy in the Route 53 console.
* Setup DNS Nameservers on GoDaddy to point to Route 53. [See Making Route 53 the DNS service for a domain that's in use](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/migrate-dns-domain-in-use.html)
* Follow the [AWS Route 53 Docs](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-to-cloudfront-distribution.html#routing-to-cloudfront-distribution-config) for Configuring Amazon Route 53 to route traffic to a CloudFront distribution.
  * Create the **A** Record.  Note: make sure that the **Record Name** in the Record matches the **Alternate domain names** in the Cloudfront Distribution. If the domain name in Cloudfront is ```*.example-site.com``` then the **A** Record must be the same. [See this Stack Overflow Answer](https://stackoverflow.com/questions/30611917/cloudfront-distribution-not-showing-as-route53-alias-target#:~:text=Combining%20several%20correct,status%20is%20Deployed.)
  * Creat the **AAAA** Record the same way you setup the **A** Record.


S3 Endpoint
http://journalmakr.com.s3-website.us-east-2.amazonaws.com/

CloudFront Endpoint
https://d3c2smw6z0koka.cloudfront.net

AWS CloudFront - Distributions console
https://console.aws.amazon.com/cloudfront/v3/home?#/distributions

AWS S3 console
https://s3.console.aws.amazon.com/s3/home?region=us-east-2&region=us-east-2

GoDaddy Journalmakr dns
https://dcc.godaddy.com/manage/journalmakr.com/dns?plid=1

## Notes & Research

### Notes & Research - localhost
* [Got a simple webpage you want to host on localhost?](https://dev.to/nickraphael/got-a-simple-webpage-you-want-to-host-at-localhost-1eke)
* TODO [Grunt - The JavaScript Task Runner](https://gruntjs.com/)
* [Install Node.js and npm using Homebrew on OS X and macOS](https://changelog.com/posts/install-node-js-with-homebrew-on-os-x)
* [Understanding client-side web development tools - Learn web development | MDN](https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Understanding_client-side_tools)

### Notes & Research - GitHub
* [GitHub Pages Documentation](https://docs.github.com/en/pages)
* [GitHub Desktop Documentation](https://docs.github.com/en/desktop). Step-by-step guides to set up and use GitHub Desktop to manage your project work.
* [Using GitHub to host a free static website](https://www.geeksforgeeks.org/using-github-to-host-a-free-static-website/)

### Notes & Research - AWS
* [Understanding and getting your AWS credentials](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys)

### Notes & Research - AWS S3
* [Tutorial: Configuring a static website on Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/HostingWebsiteOnS3Setup.html)
* [AWS Website Endpoints](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteEndpoints.html)
* [Creating a Bucket on AWS S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/create-bucket-overview.html)
* [Configuring block public access settings for your S3 buckets](https://docs.aws.amazon.com/AmazonS3/latest/userguide/configuring-block-public-access-bucket.html)
* [Uploading Objects](https://docs.aws.amazon.com/AmazonS3/latest/userguide/upload-objects.html)

### Notes & Research - AWS CloudFront
* [Use CloudFront to serve a static website hosted on Amazon S3](https://aws.amazon.com/premiumsupport/knowledge-center/cloudfront-serve-static-website/)
* [How do I use CloudFront to serve a static website hosted on Amazon S3?](https://aws.amazon.com/premiumsupport/knowledge-center/cloudfront-serve-static-website/)
* [Using custom URLs by adding alternate domain names (CNAMEs)](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/CNAMEs.html)

### Notes & Research - AWS Certificate Manager
* [DNS Validation](https://docs.aws.amazon.com/acm/latest/userguide/dns-validation.html#cnames-overview)
* [DNS validation on GoDaddy fails](https://docs.aws.amazon.com/acm/latest/userguide/troubleshooting-DNS-validation.html#troubleshooting-DNS-GoDaddy)
* [AWS Certificate Manager FAQs](https://aws.amazon.com/certificate-manager/faqs/)

### Notes & Research - AWS Route 53
* [Making Route 53 the DNS service for a domain that's in use](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/migrate-dns-domain-in-use.html)
* [Routing traffic to an Amazon CloudFront distribution by using your domain name](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-to-cloudfront-distribution.html#routing-to-cloudfront-distribution-config)
* [Configuring Amazon Route 53 as your DNS service - Amazon Route 53](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/dns-configuring.html)

### Notes & Research - AWS CodePipeline
* [Automate static website deployment from Github to S3 using AWS CodePipeline](https://medium.com/avmconsulting-blog/automate-static-website-deployment-from-github-to-s3-using-aws-codepipeline-16acca25ebc1)
* [Automate static website deployment from Github to S3 using AWS CodePipeline | by Sithum Jayarathna | AVM Consulting Blog | Medium](https://medium.com/avmconsulting-blog/automate-static-website-deployment-from-github-to-s3-using-aws-codepipeline-16acca25ebc1)

### Notes & Research - GoDaddy
* [Add a CNAME record](https://www.godaddy.com/help/add-a-cname-record-19236)

### Notes & Research - Github Actions
* TODO [S3 Sync · Actions · GitHub Marketplace](https://github.com/marketplace/actions/s3-sync)
* TODO [Workflow syntax for GitHub Actions - GitHub Docs](https://docs.github.com/en/actions/learn-github-actions/workflow-syntax-for-github-actions)


### Notes & Research - Option 3
* [Boilerplate Template - index.html](https://www.freecodecamp.org/news/basic-html5-template-boilerplate-code-example/)
* [Boilerplate Template - error.html](https://github.com/h5bp/html5-boilerplate/blob/main/src/404.html)
* [Host your static website in AWS under 2 minutes with S3](https://medium.com/avmconsulting-blog/host-your-static-website-in-aws-under-2-minutes-with-s3-a01237a83fda)
* [How to build a Hugo website in AWS Lambda and deploy it to S3](https://bezdelev.com/hacking/hugo-aws-lambda-static-website-amazon-s3/).  See also a follow-up post [Breakdown of costs for a static website on AWS after 18 months in production](https://medium.com/@bezdelev/cost-breakdown-for-a-static-website-on-aws-after-18-months-in-production-d97a932d2d25).
* [Creating a static website using GoDaddy, AWS S3, and AWS CloudFront](https://dale-bingham-soteriasoftware.medium.com/creating-a-static-website-using-godaddy-github-aws-s3-codedeploy-and-aws-cloudfront-1990a8f4ddd8)
* [Setting Up a GoDaddy Domain Name with Amazon Web Services](http://www.mycowsworld.com/blog/2013/07/29/setting-up-a-godaddy-domain-name-with-amazon-web-services)
* [Host a Static Site on AWS, using S3 and CloudFront](https://www.davidbaumgold.com/tutorials/host-static-site-aws-s3-cloudfront/#make-a-cloudfront-distribution)
* [Domain by GoDaddy, DNS by Route53](https://jryancanty.medium.com/domain-by-godaddy-dns-by-route53-fc7acf2f5580)
