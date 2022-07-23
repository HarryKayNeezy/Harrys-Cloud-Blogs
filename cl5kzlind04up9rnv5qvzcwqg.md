## Hosting A Static Website on AWS Using Amazon S3 and Cloudfront.

Amazon Simple Storage Service (Amazon S3) is an object storage service that offers cutting-edge durability, scalability, data availability, security and performance at very low costs.

We will be hosting a static website on Amazon S3 and accessing the cached website pages using Amazon CloudFront content delivery network (CDN) service. 

Amazon CloudFront is a web service that gives businesses and web application developers an easy and cost effective way to distribute content with low latency and high data transfer speeds. Amazon CloudFront offers low latency and high transfer speeds during website rendering. 

In this project, we will deploy a static website to AWS by creating a public S3 bucket and uploading the website files to our S3 bucket. We will then configure the bucket for website hosting and secure it using IAM policies. We will speed up content delivery using AWS content distribution network service, CloudFront. Finally, we will access our website in a browser using the unique CloudFront endpoint.

You need prior access to an Amazon Web Services(AWS) account. If you are completely new to Amazon Web Services, you can sign up and create a Free Tier account in minutes. [Sign up here](https://portal.aws.amazon.com/billing/signup#/start/email)

#### Create an S3 Bucket


<ol>
<li> After creating your AWS account, you will be directed to the AWS Management Console page, type “S3” in the “Search for Services” box and then select “S3”.

![Screenshot 2022-07-13 at 12.37.12 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1657715869457/kufIqKTaW.png align="left") 

<li>The Amazon S3 dashboard displays. Click on “Create bucket”.

 ![Screenshot 2022-07-13 at 12.40.03 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1657716084234/q1X4-4t3M.png align="left")

<li> The Create bucket wizard opens. Enter a “Bucket name” and a region of your choice. The bucket name must be unique across all of Amazon S3 and only contain with lowercase characters.


![Screenshot 2022-07-13 at 12.54.49 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1657717088997/OnoAnwvtI.png align="left")

<li> In the Bucket settings for Block Public Access section, uncheck the “Block all public access”. It will enable the public access to the bucket objects via the S3 object URL. 
Check the "I acknowledge that the current settings might result in this bucket and the objects within becoming public" in order to proceed. <br>
 > Note - We are allowing the public access to the bucket contents because we are going to host a static website in this bucket. Hosting requires the content should be publicly readable.


![Screenshot 2022-07-13 at 12.55.20 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1657717105309/q-jiwn1ZC.png align="left")

<li> Click “Next” and click “Create bucket”.
> You've created a bucket in Amazon S3.


![Screenshot 2022-07-13 at 12.56.48 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1657717132097/Gc_O4h9CV.png align="left")

<li> Once the bucket is created, click on the name of the bucket to open the bucket to the contents.


![Screenshot 2022-07-13 at 12.57.49 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1657717144849/u6BBP180a.png align="left")

</ul>



<h4> Upload files to your S3 Bucket </h4>
I have prepared a free template that we use as our static website for this project, which can be found on my Github [repo - click here](https://github.com/HarryKayNeezy/S3-Static-Website-Hosting.git).
<ol>
<li>
Click on "Download zip" to download the files to your local computer and extract the website files to be uploaded to our S3 bucket.

![Screenshot 2022-07-13 at 1.33.10 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1657719284971/Cl1pXPJrW.png align="left")

<li> Now we are ready to upload the local files and folders to our S3 bucket.
Click on the "Upload" button.
<li>Click the "Add files" and “Add folder” button, and upload the Static-Website-Hosting code folder content from your local computer to the S3 bucket. (You can also upload your own code - your own preference.)


![Screenshot 2022-07-13 at 1.56.38 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1657721129757/GrmswuUkZ.png align="left")

<li>Click "Add files" to upload the index.html file and components.html inside the public html, and click "Add folder" to upload the css, imgs, js, scss and vendors folders inside the assets folder.


![Screenshot 2022-07-13 at 2.01.56 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1657721148562/KeRmRk-9N.png align="left")

Do not select the entire static-website-hosting folder. Instead, upload its content one-by-one.


![Screenshot 2022-07-13 at 2.16.00 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1657723478663/IdzDC9T7x.png align="left")
<br>

<h4> Secure your S3 Bucket via IAM </h4>
<ol>
<li> Click on the “Permissions” tab.
<li> Under the “Bucket Policy” section . Click on the Edit button.
<li> Enter the following bucket policy replacing "your-bucket-name" with the name of your bucket and click “Save”. <br>



![Screenshot 2022-07-13 at 2.54.58 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1657724196342/snLKy2FOb.png align="left")

<code class="nohighlight hljs"><span>{</span>
   <br> "Version": "2012-10-17",
     <br>"Statement": [
         <br><span>{</span>
             <br>"Sid": "PublicReadGetObject",
             <br>"Effect": "Allow",
             <br>"Principal": "*",
             <br>"Action": [
                 <br>"s3:GetObject"
            <br> ],
             <br>"Resource": [
                 <br>"arn:aws:s3:::<code class="replaceable">Your-Bucket-Name</code>/*"
             <br>]
         <br>}
     <br>]
 <br>}</code>

 <blockquote>
        <p> Copy the bucket policy directly from my GitHub project page to avoid indentation errors. ->[here - awsbucketpolicy](https://github.com/HarryKayNeezy/S3-Static-Website-Hosting.git)</p>
    </blockquote>
</ol>

<h4> Configure S3 Bucket </h4>
<ol>
<li>Go to the Properties tab and then scroll down to edit the Static website hosting section.
![Screenshot 2022-07-13 at 9.39.17 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1657748529238/WjDhuxLj9.png align="left")

<li>Click on the “Edit” button to see the Edit static website hosting screen. Now, enable the Static website hosting, and provide the default home page and error page for your website.

![Screenshot 2022-07-13 at 9.40.28 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1657748555775/sOi9tlgn0.png align="left")

<li>For both “Index document” and “Error document”, enter “index.html” and click “Save”. After successfully saving the settings, check the Static website hosting section again under the Properties tab. You must now be able to view the website endpoint as shown below:
![Screenshot 2022-07-13 at 9.41.39 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1657748594758/vww2wQNEM.png align="left")

<li> Click on the bucket website endpoint to view your website page hosted on Amazon S3.

![Screenshot 2022-07-13 at 9.50.00 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1657749030813/Y_nPhff75.png align="left")
 
<blockquote>
        <p>Congratulations you have hosted your static website on Amazon S3</p>
 </blockquote>
</ol>

<h4> Distribute Website via CloudFront </h4> 
<ol>
<li> Select “Services” from the top left corner of your AWS navbar(navigation bar) and enter “Cloudfront” in the “Search for services, features, ...” text box and select “CloudFront”.

<li> On the CloudFront dashboard, click “Create a Cloudffront Distribution”.
![Screenshot 2022-07-13 at 9.56.42 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1657749417972/WLkDFPmjV.png align="left")

<li> On the Create Distribution Dashboard, Enter the Origin Domain name by pasting your bucket static website hosting endpoint into the Origin text box. 
<p> Do not select the bucket from the dropdown list. Instead, paste the Static website hosting endpoint of your bucket such as `<bucket-name>.s3-website-<region>.amazonaws.com`. My endpoint is `http://my-static-bucket-1993.s3-website-us-east-1.amazonaws.com`. See below: </p>

![Screenshot 2022-07-13 at 9.59.04 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1657749843756/A1s84y-6C.png align="left")

<li> Leave the defaults for the rest of the options, and click “Create Distribution”. It may take up to 10 minutes for the CloudFront Distribution to get created.

<li> Once the status of your distribution changes from “In Progress” to “Deployed”, copy the endpoint URL for your CloudFront distribution found in the “Domain Name” column.

![Screenshot 2022-07-13 at 10.16.17 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1657750601669/0wbfwOIlw.png align="left")

<blockquote>
    <p> My Domain Name value is `d3gmbl55xb907r.cloudfront.net`, but yours will be different.</p>
</blockquote>
</ol>


<h4> Access Website in Web Browser </h4>
<blockquote>
    <p> Note - In the steps below, the exact domain name and the S3 URLs might be different in your case. </p>
</blockquote>
</ol>
<ol>
<li>Open a web browser like Google Chrome, and paste the copied CloudFront domain name (such as, `d3gmbl55xb907r.cloudfront.net`) without appending /index.html at the end. The CloudFront domain name should show you the content of the default home-page, as shown below:

![Screenshot 2022-07-13 at 10.20.31 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1657750846459/vQe5gPgwP.png align="left")

<blockquote>
    <p> The image above shows the hosted static website page displayed at `https://d3gmbl55xb907r.cloudfront.net` </p>
</blockquote>

<li> Access the website via website-endpoint, such as `http://<bucket-name>.s3-website.us-east-1.amazonaws.com/` 
<p> (Mine is `http://my-static-bucket-1993.s3-website-us-east-1.amazonaws.com`) </p>


![Screenshot 2022-07-13 at 10.29.18 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1657751386086/4uaZ4Dt1W.png align="left")

<li> Access the bucket object via its S3 object URL, such as, `https://<bucket-name>.s3.amazonaws.com/index.html`
<p> (Mine is `http://my-static-bucket-1993.s3.amazonaws.com/index.html`) </p>

![Screenshot 2022-07-13 at 10.28.18 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1657751318816/3HfpKwIQO.png align="left")


All three links: CloudFront domain name, S3 object URL, and website-endpoint will show you the same `index.html` content.
</ol>

<h5> Congratulations you have hosted your static webpage on Amazon S3 and successfully done the following;
<ul>
<li> Created and configured an S3 bucket for website hosting, and secured it using IAM policies.
<li> Uploaded the website files to your bucket and sped up content delivery using AWS’s content distribution network service, CloudFront.
<li> Accessed your website in a browser using the unique S3 endpoint and Cloudfront domain name.
 </h5>

Read more on Amazon S3 and Amazon CloudFront on the Amazon Web Services pages including the Frequently Asked Questions (FAQs) to gain more insights into these AWS services.


<blockquote>
    <p> Reminder - Delete/shutdown every AWS resource (e.g., S3 buckets, Cloudfront distribution) that you will not be no longer using. </p>
</blockquote>
