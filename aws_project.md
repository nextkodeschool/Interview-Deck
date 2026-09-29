## Project Explanation

> I’m currently working on a **Food and Retail domain project**. The application is called a **Trade and Stock Dashboard**, which is used to manage and monitor information such as products, inventory, stock availability, trade transactions, and other retail operational data.
>
> The application follows a **microservices architecture**, so instead of having one large monolithic application, the functionality is split into multiple independently deployable services.
>
> From the DevOps side, our technology stack mainly includes **Linux, Shell Scripting, Git, GitHub, GitHub Actions, Terraform, Docker, Kubernetes, and AWS services**.
>
> Our main responsibility is to provide the complete automation around the application, starting from infrastructure provisioning, code integration, image creation, deployment to Kubernetes, environment promotion, and production support.
>
> We maintain **separate repositories and separate pipelines for infrastructure and application deployment**. This separation is important because infrastructure changes and application changes have different lifecycles.

### Infrastructure Pipeline

> For infrastructure, we maintain a dedicated **Infrastructure GitHub repository**.
>
> This repository contains reusable **Terraform modules**, Terraform resource files, variables, outputs, environment-specific `.tfvars` files, and GitHub Actions workflows.
>
> We use a common Terraform codebase for all environments. We don’t maintain completely separate Terraform code for Dev, QA, Pre-prod, and Production.
>
> Instead, we maintain environment-specific variable files such as:
>
> `dev.tfvars`, `qa.tfvars`, `preprod.tfvars`, and `prod.tfvars`.
>
> Our Git branching strategy is aligned with the environments.
>
> We have:
>
> `dev` for Development,  
> `qa` for QA,  
> `release` for Pre-production,  
> and `main` for Production.
>
> when we make Terraform changes in feature branches and raise a Pull Request to the Dev branch.
>
> When the change is merged into Dev, the GitHub Actions infrastructure pipeline is triggered.
>
> The pipeline performs steps such as `terraform init`, `terraform validate`, `terraform plan`, and then `terraform apply` using `dev.tfvars`.
>
> So the change is first applied to the Development infrastructure.
>
> Once it is validated, the same change is promoted from Dev to QA through another PR.
>
> When the change reaches the QA branch, the pipeline uses `qa.tfvars` and applies the infrastructure changes to the QA environment.
>
> The same promotion model continues through Release and Main, using `preprod.tfvars` and `prod.tfvars`.
>
> So the key idea is that **the Terraform code remains reusable, while the environment-specific configuration is controlled through tfvars files and branch-based automation**.

### Build and Release Pipeline

> The second major pipeline is our **application Build and Release pipeline**.
>
> Each microservice has its own application code and Dockerfile, along with the required Kubernetes deployment configuration.
>
> Developers work using feature branches such as `feature/login`, `feature/inventory`, `feature/orders`, or other service-specific branches.
>
> Once the development work is completed, they raise a Pull Request toward the Dev branch.
>
> Before allowing the merge, we execute quality and validation checks.
>
> We use **SonarQube** for code quality analysis, along with security checks, linting, and other validation steps.
>
> Only when the required checks pass and the PR is approved is the code merged into the Dev branch.
>
> Once the merge happens, the **Dev GitHub Actions pipeline** is automatically triggered.

### Docker Build and Registry

> The pipeline first checks out the code and uses the Dockerfile available in the repository to build a Docker image.
>
> We use proper image tagging so that every build can be uniquely identified.
>
> The Docker image is then pushed to **Amazon ECR**, which we use as our container registry.
>
> This is an important distinction: **ECR stores the Docker images; we are not pushing the images to ECS.**
>
> Our actual application deployment target is Kubernetes.

### Kubernetes Deployment

> Once the Docker image is successfully pushed to ECR, the release or deployment stage is triggered.
>
> The release pipeline updates the Kubernetes deployment to use the required image and deploys the application into the corresponding environment namespace.
>
> For example, when the code is merged into the Dev branch, it is deployed into the **Dev namespace**.
>
> Similarly, when the code is promoted from Dev to QA, the QA pipeline is triggered and the application is deployed into the **QA namespace**.
>
> We follow the same model for Pre-production and Production.
>
> So effectively, our branch-to-environment mapping is:
>
> `dev` → Development  
> `qa` → QA  
> `release` → Pre-production  
> `main` → Production.

### Environment Promotion

> The overall promotion flow is:
>
> **Feature → Dev → QA → Release → Main.**
>
> Every environment promotion happens through a Pull Request and approval process rather than directly changing the higher environment branches.
>
> This gives us better traceability, auditability, and controlled releases.
>
> It also makes troubleshooting easier because we know exactly which commit, Docker image, and deployment went into each environment.

### My Responsibilities

> My responsibilities in this project include maintaining and improving the Git branching and PR workflows, developing and supporting GitHub Actions pipelines, maintaining Terraform modules and infrastructure code, managing environment-specific tfvars, supporting Docker builds and image publishing to ECR, and deploying applications into Kubernetes.
>
> I’m also involved in troubleshooting failed builds and deployments, resolving Terraform and Kubernetes-related issues, writing Linux and Shell automation scripts, supporting release activities, and coordinating with application teams when there are deployment issues.
>
> At a Senior or Lead level, I’m also involved in reviewing infrastructure and pipeline changes, improving reusable automation, reducing manual steps, standardizing deployment processes across environments, and helping teams troubleshoot production and release issues.

### Why We Use Separate Infrastructure and Application Pipelines

> One design decision in our project is that we keep infrastructure and application pipelines separate.
>
> Infrastructure changes are less frequent and usually require Terraform plan, review, approvals, and controlled apply operations.
>
> Application changes happen more frequently and normally require code validation, Docker image creation, image publishing, and Kubernetes deployment.
>
> Keeping these pipelines separate gives us better control, independent versioning, clearer ownership, easier troubleshooting, and reduced deployment risk.

### Closing

> So in summary, **Terraform provisions the AWS infrastructure, GitHub Actions automates the CI/CD process, Docker packages the application, Amazon ECR stores the container images, and Kubernetes runs the microservices across Dev, QA, Pre-prod, and Production environments.**
>
> My role is mainly around designing, maintaining, troubleshooting, and improving this overall DevOps automation and release process.

---

## Easy 7-Part Memory Structure

Before an interview, just remember:

**1. Project** — Food & Retail Trade and Stock Dashboard  
**2. Architecture** — Microservices on AWS  
**3. Tools** — GitHub Actions, Terraform, Docker, Kubernetes, ECR  
**4. Pipeline 1** — Terraform infrastructure pipeline  
**5. Pipeline 2** — Sonar → Docker → ECR → Kubernetes  
**6. Promotion** — Feature → Dev → QA → Release → Main  
**7. My Role** — Build, automate, deploy, troubleshoot, improve

One useful improvement for a 7–10 year interview is to spend **only about 3–4 minutes describing the flow**, then spend the remaining time on **your ownership, one real challenge, and one improvement you implemented**. That usually sounds more senior than spending the full seven minutes only explaining tools and pipelines.
