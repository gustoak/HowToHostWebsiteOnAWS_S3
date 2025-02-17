# Hosting a Website on Amazon S3 using GitHub

This guide will show you how to host a static website on Amazon S3 using files stored in a GitHub repository.

## Prerequisites

Before you begin, ensure you have the following:
- An Amazon Web Services (AWS) account
- A GitHub repository with your website files (HTML, CSS, JS, etc.)
- Basic knowledge of Amazon S3 and AWS Console

---

## Step 1: Create an S3 Bucket

1. Go to the [Amazon S3 Console](https://console.aws.amazon.com/s3/).
   
   [insert image: AWS S3 Console]

2. Click on the **Create bucket** button.

3. Provide a globally unique name for your bucket (e.g., `my-website-bucket`).

4. Choose a region for your bucket (choose a region close to your audience).

5. Uncheck **Block all public access** to allow your website to be publicly accessible.
   
   > **Warning:** This will make your bucket publicly accessible. Ensure that the contents are intended to be publicly available.

6. Click on **Create bucket**.

   [insert image: Create S3 Bucket]

---

## Step 2: Enable Static Website Hosting

1. After your bucket is created, go to your bucket's properties.

2. Scroll down and click on **Static website hosting**.

3. Select **Use this bucket to host a website**.

4. In the **Index document** field, enter `index.html` (or whatever your homepage file is named).

5. Optionally, enter an **Error document** (e.g., `error.html`).

6. Click **Save changes**.

   [insert image: Enable Static Website Hosting]

---

## Step 3: Upload Website Files to S3

1. Go to your bucket and click on the **Upload** button.

2. Click on **Add files** and select all the website files from your local machine or GitHub.

3. Once the files are selected, click **Upload**.

   [insert image: Upload Files to S3]

---

## Step 4: Make Your Files Public

1. Select all the files in your S3 bucket.

2. Click on the **Actions** dropdown and choose **Make public**.

3. Confirm that you want to make the files public.

   [insert image: Make Files Public]

---

## Step 5: Link Your GitHub Repository to S3

To automate deploying from GitHub, we can set up GitHub Actions.

1. Go to your GitHub repository.

2. Under the **Actions** tab, create a new workflow by clicking on **New workflow**.

3. Add the following YAML configuration for GitHub Actions:

```yaml
name: Deploy Website to S3

on:
  push:
    branches:
      - main  # Trigger on push to the main branch

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Setup AWS CLI
        uses: aws-actions/configure-aws-credentials@v1
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-1  # Change to your desired region

      - name: Sync files to S3
        run: |
          aws s3 sync . s3://<your-bucket-name> --exclude ".git/*" --acl public-read
