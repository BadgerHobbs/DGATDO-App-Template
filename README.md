# DGATDO-App-Template
A demo/template repository for building and deploying an application using Docker, GitHub Actions, Terraform, and DigitalOcean. Get started quickly with this template and leverage the power of these technologies to streamline your application deployment process.
A demo/template repository for building and deploying an application using Docker, GitHub Actions, Terraform, and Digital Ocean. Get started quickly with this template and leverage the power of these technologies to streamline your application deployment process.

# Manual Deployment

## Create GitHub Access Token and Login to GitHub Container Registry
Go to GitHub and create a personal access token with repository read/write permissions, as documented [here](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry).

Use the following command to login using your username and token.

```bash
docker login ghcr.io -u <USERNAME> -p <GITHUB_PAT>
```

## Build, Tag, and Push Docker Image
Run the following commands to build the application Docker image, then tag it, then push it to the GitHub container registry.

```bash
docker build -t dgatdo-application:latest -f Docker/Dockerfile .
```

```bash
docker tag dgatdo-application:latest ghcr.io/<USERNAME>/dgatdo-application:latest
```

```bash
docker push ghcr.io/<USERNAME>/dgatdo-application:latest
```

## Create SSH key for Deployment
Use the following commands to generate an SSH key for deployment. It will be passed to the deployed application/container in order to run commands to login to the GitHub container registry and pull the latest image.

```bash
ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa
```

## Create DigitalOcean Access Token
Go to DigitalOcean and create a new access token, as documented [here](https://docs.digitalocean.com/reference/api/create-personal-access-token/).

## Set Terraform Variables
Copy and rename the `template.tfvars` file to `local.tfvars` and update the values.

## Intialise and Deploy using Terraform
Run the following commands to deploy the application using Terraform, making sure to answer `yes` to confirmation prompts for `plan` and `deploy`.

```bash
terraform -chdir="./Terraform" init
```

```bash
terraform -chdir="./Terraform" plan -var-file="local.tfvars"
```

```bash
terraform -chdir="./Terraform" apply -var-file="local.tfvars"
```

## Destroy Deployment using Terraform
Run the following command to destroy your previously deployed application using Terraform, making sure to answer `yes` to confirm destruction.

```bash
terraform -chdir="./Terraform" destroy -var-file="local.tfvars"
```
