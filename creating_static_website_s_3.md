# 🚀 Project 2: Creating a Static Website for the Cafe

## 🌟 Project Goal

The goal of this project is to host a static website for a cafe using Amazon S3, apply public access settings, enable versioning, configure lifecycle rules, and implement cross-region replication.

---

## 🛠️ AWS Console Steps

### 📁 Step 1: Download Website Files

- Open your GitHub repository.
- Download the website files.
- Extract the folder on your PC.

---

### 🌐 Step 2: Create S3 Bucket

1. Go to AWS Console → search for `S3`.
2. Click **Create bucket**.
3. Set a **unique bucket name**.
4. Enable **ACLs**.
5. Uncheck **Block all public access**.
6. Click **Create bucket**.

---

### 📂 Step 3: Upload Website Files

1. Open the bucket → click **Upload**.
2. Upload:
   - `index.html`
   - `CSS` folder
   - `Image` folder
3. Scroll down → click **Upload**.

---

### 🛏️ Step 4: Enable Static Website Hosting

1. Inside the bucket → go to **Properties**.
2. Scroll to **Static website hosting** → click **Edit**.
3. Choose **Enable**.
4. Select **Host a static website**.
5. In **Index document**, write: `index.html`.

---

### 🔒 Step 5: Make Website Public

To grant public read-only access:

1. Go to **Permissions** tab → scroll to **Bucket Policy** → click **Edit**.
2. Paste the following policy (replace `S3bucket` with your actual bucket name):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::S3bucket/*"
    }
  ]
}
```

🔗 [View AWS Documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/HostingWebsiteOnS3Setup.html#step4-add-bucket-policy-make-content-public)

3. Save changes.

---

### 🔄 Step 6: Enable Versioning

1. Go to **Properties** of the S3 bucket.
2. Scroll to **Bucket Versioning** → click **Edit**.
3. Choose **Enable** → Save.

---

### ✏️ Step 7: Update the Website File

1. Open `index.html` locally.
2. Make changes:
   - Replace `bgcolor="aquamarine"` → `bgcolor="gainsboro"`
   - Replace `bgcolor="orange"` → `bgcolor="cornsilk"`
   - Save and close the file.
3. Upload the updated file to the S3 bucket.
4. Go to bucket → click **Show versions** → confirm multiple versions exist.

---

### 📃 Step 8: Create Lifecycle Rules

#### Rule 1: Move Previous Versions After 30 Days

- Go to **Management** → **Lifecycle rules** → **Create rule**
- Rule name: `MovePreviousVersionsAfter30Days`
- Scope: Apply to all objects
- Action: Transition noncurrent versions to S3 Standard-IA

#### Rule 2: Delete Previous Versions After 365 Days

- Create another rule:
- Rule name: `DeletePreviousVersionsAfter365Days`
- Scope: Apply to all objects
- Action: Permanently delete noncurrent versions

> 🔹 The first rule transitions old versions to cheaper storage. 🔹 The second rule deletes them after 1 year.

---

### 🌍 Step 9: Cross-Region Replication

You want to replicate the site from Virginia to another region (e.g., Ohio).

#### Step A: Create a New Bucket in Ohio

1. Switch to **Ohio region**.
2. Create new S3 bucket.
3. Enable ACLs, clear block public access, and enable versioning.

#### Step B: Setup Replication From Source Bucket

1. Go back to Virginia → open original bucket.
2. Go to **Management** → **Replication rules** → **Create replication rule**
   - Rule name: `ReplicateToOtherRegion`
   - Status: Enabled
   - Scope: Apply to all objects
   - Destination: Select Ohio bucket
   - IAM Role: Use existing (e.g., `CafeRole`) or create new
3. Save rule.

#### Step C: Upload New Version

1. Modify `index.html` again locally.
2. Upload it to the Virginia bucket.
3. Confirm 3 versions exist.
4. Check the Ohio bucket → the file should appear.

#### Step D: Delete Latest Version in Virginia

1. Delete the latest version of `index.html` in Virginia.
2. Go to Ohio → the file is still there → **not deleted**.

> ℹ️ Replication doesn’t delete files in the destination region when removed from the source.

---

## ✅ Project Completed

- Website hosted on Amazon S3 with public access.
- Versioning and lifecycle rules configured.
- Cross-region replication implemented.

See you in the next project!

Goodbye 👋

