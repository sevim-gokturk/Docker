# AWS EC2 Setup with Terraform

## 1. Create a User Group with EC2 Full Access

1. Go to AWS Management Console.
2. Open **IAM (Identity and Access Management)**.
3. Click **User Groups** > **Create Group**.
4. Name the group (e.g., `EC2FullAccessGroup`).
5. Attach policy **AmazonEC2FullAccess**.
6. Click **Create Group**.

## 2. Create a User and Add to the Group

1. In IAM, go to **Users** > **Add User**.
2. Enter a username (e.g., `terraform-user`).
3. Select **Provide user access to the AWS Management Console - optional**.
4. Select **Access Key - Programmatic Access** if available, or proceed with default settings.
5. Click **Next** and add the user to `EC2FullAccessGroup`.
6. Click **Create User**.

## 3. Give Permissions (If Not Assigned)

1. Go to **IAM > Users**.
2. Select the user.
3. Attach **AmazonEC2FullAccess** policy if not already assigned.

## 4. Create an Access Key

1. In **IAM > Users**, select your user.
2. Go to the **Security credentials** tab.
3. Click **Create Access Key**.
4. Copy and save **Access Key ID** and **Secret Access Key**.

## 5. Configure AWS CLI Locally

```sh
aws configure
```

- Enter **Access Key ID**.
- Enter **Secret Access Key**.
- Choose **Region** (e.g., `us-east-1`).
- Output format: `yaml`.

## 6. Create a Key Pair (PEM File) from AWS Console

1. Go to **EC2 Dashboard**.
2. Click **Key Pairs** under **Network & Security**.
3. Click **Create Key Pair**.
4. Enter a key name (e.g., `my-key`).
5. Choose **PEM** format.
6. Click **Create Key Pair** and download the `.pem` file.
7. Move the key to a secure location and set permissions:

```sh
chmod 400 my-key.pem
```

## 7. Prepare Terraform Configuration File

Create a Terraform configuration file (`.tf` file) that defines your AWS resources. Ensure it includes the necessary provider, resource, and instance details.(Day5_compose\ec2.tf)

## 8. Initialize and Apply Terraform

```sh
terraform init
terraform apply -auto-approve
```

Now your EC2 instance is deployed!

## NOT: When you done with your projet, dont forget to destroy all that you created on AWS.
```sh
terraform destroy
```
