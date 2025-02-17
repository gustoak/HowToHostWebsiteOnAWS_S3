# Host a Website on Amazon S3 from GitHub

This guide will walk you through the steps to host a static website on Amazon S3 using an existing GitHub repository. We'll cover creating an S3 bucket, configuring the bucket for static hosting, uploading website files, and automating deployment using GitHub Actions.

---

## Step 1: Set Up an S3 Bucket for Website Hosting

### 1.1 Create an S3 Bucket
1. Go to the [**Amazon S3 Console**](https://console.aws.amazon.com/s3/).
2. Click on the **Create bucket** button.

   ![Create Bucket](https://www.example.com/criar_bucket.png) *(Replace with the real screenshot)*

3. Choose a **unique name** for your bucket (e.g., `mywebsite-bucket`). The bucket name must be globally unique.
4. Select the **region** for the bucket (e.g., `US East (N. Virginia)`).
5. Leave other options as default unless needed (e.g., versioning, logging).
6. Click on **Create bucket**.

   ![Bucket Creation Screen](https://www.example.com/tela_criacao_bucket.png) *(Replace with the real screenshot)*

---

### 1.2 Enable Static Website Hosting
1. After the bucket is created, click on it to open it.
2. Navigate to the **Properties** tab.

   ![Bucket Properties Tab](https://www.example.com/propriedades_bucket.png) *(Replace with the real screenshot)*

3. Scroll down to the **Static website hosting** section and click **Edit**.
4. Select **Enable** and choose **Host a static website**.
5. In the **Index document** field, type `index.html`.
6. In the **Error document** field, type `error.html` (optional).
7. Click **Save changes**.

   ![Static Website Hosting Settings](https://www.example.com/configuracoes_hospedagem_estatica.png) *(Replace with the real screenshot)*

---

## Step 2: Make the Bucket Public

To allow public access to your files, ensure the bucket is publicly accessible.

### 2.1 Modify Bucket Policy to Allow Public Access
1. Navigate to the **Permissions** tab of your bucket.
2. Under **Block public access**, click **Edit** and uncheck **Block all public access** (if checked).
3. Confirm changes by clicking **Save changes**.

   ![Public Access Settings](https://www.example.com/configuracoes_acesso_publico.png) *(Replace with the real screenshot)*

### 2.2 Set Bucket Policy
1. Under the **Permissions** tab, click on **Bucket Policy**.
2. Add the following policy to allow public access to all files:

   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Sid": "PublicReadGetObject",
         "Effect": "Allow",
         "Action": "s3:GetObject",
         "Resource": "arn:aws:s3:::mywebsite-bucket/*"
       }
     ]
   }
