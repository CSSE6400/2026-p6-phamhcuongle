[![Open in Codespaces](https://classroom.github.com/assets/launch-codespace-2972f46106e565e64193e422d61a12cf1da4916b45550586e14ef0a7c637dd04.svg)](https://classroom.github.com/open-in-codespaces?assignment_repo_id=23357168)
# TaskOverflow AWS Deployment

This repository contains the Terraform infrastructure code to deploy the **TaskOverflow** application to AWS. 
The resources created include an Application Load Balancer, an Elastic Container Service (ECS) cluster and task definition, Auto Scaling, and a PostgreSQL database using AWS RDS.

## Architecture

* **ECS:** Runs the TaskOverflow container image (`ghcr.io/csse6400/taskoverflow:latest`).
* **RDS:** A PostgreSQL database backend used by the API.
* **Auto Scaling:** Automatically scales the number of task instances based on load.
* **Load Balancer:** An Application Load Balancer (ALB) acting as the entry point and distributing traffic across the ECS tasks.
* **Network:** Runs the applications in the specified VPC subnets and utilizes Security Groups to restrict traffic appropriately.

## Prerequisites

1. **Terraform**: Ensure you have Terraform installed to manage the infrastructure.
2. **AWS Credentials**: You will need an AWS Learner Lab `credentials` file placed in the root of the project.
3. **k6**: (Optional) For running the included load testing script.

## Setup Instructions

1. **Clone the repository:**
   ```bash
   git clone https://github.com/CSSE6400/2026-p6-phamhcuongle.git
   cd 2026-p6-phamhcuongle
   ```

2. **Configure Credentials**:
   Place your AWS Academy / Learner lab credentials file (`credentials`) in the root directory. The Terraform provider is configured to explicitly use `./credentials`.

3. **Initialize Terraform:**
   This command prepares your workspace by downloading the necessary AWS provider plugins.
   ```bash
   terraform init
   ```

4. **Review the Deployment Plan:**
   Before applying, verify the output to review all the resources that Terraform will create.
   ```bash
   terraform plan
   ```

5. **Deploy the Infrastructure:**
   Apply the configuration to provision all of the AWS resources.
   ```bash
   terraform apply
   ```
   Type `yes` when prompted.

6. **Access the Application:**
   Once `terraform apply` successfully completes, it will output the load balancer's DNS name (`taskoverflow_dns_name`).
   Navigate to that DNS name via your browser or interact with the API directly (e.g. at `/api/v1/todos`).

## Load Testing

This project includes a k6 test script (`k6.js`) for load testing and verifying your Auto Scaling setup. The script dynamically targets the Load Balancer DNS name via an environment variable.

To execute the test:
```bash
k6 run -e HOST=$(terraform output -raw taskoverflow_dns_name) k6.js
```

## Cleanup

To avoid consuming extra lab credits, be sure to always destroy the environment when you finish working:

```bash
terraform destroy
```
Type `yes` when prompted to confirm the teardown of the infrastructure.