# GitHub Self-Hosted Runners

## 1. What is a GitHub Actions runner?

A GitHub Actions runner is a machine that executes jobs in a GitHub Actions workflow.

There are two types of runners:

- **GitHub-hosted runners:** Machines managed and maintained by GitHub.
- **Self-hosted runners:** Machines that you provide and manage yourself.

A self-hosted runner can be a physical server, virtual machine, cloud instance, or even your local computer.

## 2. Why use self-hosted runners?

Self-hosted runners are useful when you need more control over where and how your CI/CD pipelines execute.

### Main reasons

1. **Private network access:** Access internal servers, private Kubernetes clusters, databases, and other resources that are not publicly accessible.
2. **Custom environment:** Install and configure specific tools, software, and dependencies.
3. **Infrastructure control:** Choose the operating system, CPU, memory, and hardware.
4. **Faster execution:** Preinstall tools and retain caches to reduce setup time.
5. **Cost optimization:** Use your own infrastructure when it is more economical than hosted runners.
6. **Specialized hardware:** Use GPUs or other hardware required by your workloads.

## 3. GitHub-hosted vs. self-hosted runners

| Feature | GitHub-hosted | Self-hosted |
|---|---|---|
| Infrastructure | Managed by GitHub | Managed by you |
| Setup | Minimal | Requires installation and configuration |
| Maintenance | GitHub handles it | You handle it |
| Software | Standard images; custom tools can be installed | Fully customizable |
| Private network access | Requires additional configuration | Can access networks it is connected to |
| Cost | Free usage limits and paid usage | Your infrastructure and maintenance costs |
| Scaling | Managed by GitHub | You manage capacity or autoscaling |

## 4. Advantages of self-hosted runners

- Greater control over the execution environment.
- Access to private infrastructure and internal resources.
- Ability to use custom software and hardware.
- Potentially faster builds through preinstalled dependencies and caching.
- Potential cost savings for frequent or resource-intensive workflows.

## 5. Disadvantages of self-hosted runners

- You are responsible for maintenance, updates, and security.
- You must manage availability and scaling.
- Persistent machines can accumulate old files, credentials, and dependencies.
- A compromised workflow can potentially access the runner's resources and credentials.
- Jobs may fail or remain queued if the runner is offline or busy.

## 6. Practical example: GitHub Actions with AWS EKS

Imagine you have a private EKS cluster on AWS and want to deploy an application using GitHub Actions.

### Workflow

1. A developer pushes code to GitHub.
2. GitHub Actions triggers the CI/CD pipeline.
3. A self-hosted runner on an EC2 instance checks out the code.
4. The runner builds and tests the application.
5. The Docker image is pushed to Amazon ECR.
6. The runner deploys the application to the private EKS cluster.

### Why use a self-hosted runner here?

The runner can be placed inside the AWS VPC, allowing it to access private AWS resources and Kubernetes endpoints.

This avoids exposing private infrastructure to the public internet merely to allow deployments.

## 7. When should you use a self-hosted runner?

Use a self-hosted runner when:

- Your deployment target is inside a private network.
- Your workflow requires custom software or hardware.
- You need control over the build environment.
- Your workload benefits from persistent caches or preinstalled dependencies.
- Your own infrastructure is more economical for your workload.

For simple build and test workflows, GitHub-hosted runners are usually easier to maintain.

## 8. Important security considerations

- Do not run untrusted pull-request workflows on a self-hosted runner with access to production infrastructure.
- Avoid storing long-lived AWS credentials directly on the runner.
- Use least-privilege IAM roles and short-lived credentials where possible.
- Keep the runner software and operating system updated.
- Use ephemeral runners for sensitive workloads when practical.
- Restrict access to the runner machine and its network.

## 9. Recommended learning path

For DevOps practice:

1. Start with GitHub-hosted runners to learn GitHub Actions workflows.
2. Configure an EC2 instance as a self-hosted runner.
3. Build and test a Docker application using the runner.
4. Push the Docker image to Amazon ECR.
5. Deploy the application to an AWS EKS cluster.
6. Practice runner security, cleanup, and scaling.

## 10. Interview answer

**Question: Why do we use self-hosted runners in GitHub Actions?**

Self-hosted runners allow us to execute GitHub Actions workflows on our own infrastructure instead of using machines managed by GitHub.

They are useful when we need custom software, specialized hardware, better control over the execution environment, or access to private resources such as internal servers and private Kubernetes clusters.

For example, we can configure an EC2 instance inside our AWS VPC as a self-hosted runner. It can build Docker images, push them to ECR, and deploy applications to a private EKS cluster.

The main trade-off is that we are responsible for maintaining, securing, and scaling the runner infrastructure.
