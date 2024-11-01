# Create a Static Website

This example demonstrates how to set up a simple static website using Amazon S3.

## Step 1: Create a Bucket

- Go to **S3 dashboard** and click on **Create bucket**.
- Other than entering an unique bucket name, uncheck the **Block all public access** option since we want public access to the files in this bucket.
  > AWS will prompt you to confirm the public access settings; acknowledge this warning if you're comfortable making the bucket public.
- Complete the setup.

## Step 2: Upload Website Content

- Open the newly created bucket by clicking its name in the S3 console.
- Upload `index.html` and the contents of `images.zip` (make sure the images are unzipped).

## Step 3: Configure Bucket Permissions

- Always from the bucket page, go to the **Permissions** tab.
- Scroll to **Bucket policy** and add the following policy, replacing `<bucket-arn>` with your actual bucket ARN (you can find the ARN at the top of the bucket's properties tab).

```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "Statement1",
			"Principal": "*",
			"Effect": "Allow",
			"Action": "s3:GetObject",
			"Resource": "<bucket-arn>/*"
		}
	]
}
```

> This policy allows public read-only access to all objects in your bucket, so they can be viewed in a web browser.

## Step 4: Enable Static Website Hosting

- Go to the **Properties** tab in your bucket.
- Scroll down to **Static website hosting** and choose **Edit**.
- Select **Enable** for static website hosting and specify **index.html** as the index document (this file will load when users visit the website's root URL).
- Once enabled, you’ll see an endpoint URL under **Bucket website endpoint**. This is your website’s public address.
- Copy the endpoint URL into a browser. You should see your static website with the main **index.html** page and images displayed correctly.

> To delete the bucket, first delete all objects within it.
