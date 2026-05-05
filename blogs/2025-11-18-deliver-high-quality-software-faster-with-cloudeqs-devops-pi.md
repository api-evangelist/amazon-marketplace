---
title: "Deliver High-Quality Software Faster with CloudEQ’s DevOps Pipeline Automation and the AWS Well-Architected Framework"
url: "https://aws.amazon.com/blogs/awsmarketplace/deliver-high-quality-software-faster-cloudeqs-devops-pipeline-automation-aws-well-architected-framework/"
date: "Tue, 18 Nov 2025 00:47:34 +0000"
author: "Priyanka Sanjeev"
feed_url: "https://aws.amazon.com/blogs/awsmarketplace/feed/"
---
<p>Organizations using manual or partially automated infrastructure often experience deployment delays that impact time-to-market. This can affect their ability to maintain consistent security and compliance processes. Organizations need both agility and governance to innovate on AWS. A multi-account strategy helps improve resource isolation, security, and compliance while helping organizations meet regulatory requirements and track costs.</p> 
<p><a href="https://aws.amazon.com/marketplace/seller-profile?id=548e62c3-265f-409a-8b4f-a4386f5ebac1">CloudEQ</a>, an AWS Partner in AWS Marketplace, addresses these challenges by integrating AWS landing zone with automated DevOps pipelines. The <a href="https://aws.amazon.com/marketplace/pp/prodview-o2sxtcl5kk4vw?sr=0-1&amp;ref_=beagle&amp;applicationId=AWSMPContessa" rel="noopener noreferrer" target="_blank">Automated DevOps Pipeline solution</a> in AWS Marketplace uses <a href="https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html" rel="noopener noreferrer" target="_blank">AWS Well-Architected Framework</a> practices to deploy secure, compliant AWS environments by combining a multi-account landing zone with infrastructure as code using Terraform and CI/CD pipelines using GitHub Actions.</p> 
<p>The AWS Well-Architected Framework helps organizations build solutions that deliver across security, performance, operations, and cost optimization. These solutions include automated monitoring and proactive issue detection to reduce operational overhead. By implementing Well-Architected solutions, organizations can focus on innovation while working with adaptable cloud infrastructure.This post explains how to accelerate software delivery and improve governance using CloudEQ’s DevOps Pipeline Automation solution.</p> 
<h1>Solution overview</h1> 
<p>When developers commit infrastructure code to GitHub, an automated CI/CD workflow is triggered. The pipeline runs a Bridgecrew security scan to identify misconfigurations and executes a Terraform plan to preview changes. After manual approval, the pipeline runs Terraform apply to provision AWS infrastructure. Terraform state files are stored in&nbsp;<a href="https://aws.amazon.com/pm/serv-s3/?trk=59968c08-333e-424a-b5a8-4fd08af5af4d&amp;sc_channel=ps&amp;ef_id=CjwKCAiAt8bIBhBpEiwAzH1w6WpItW_9VdmqO8JX-r1G8TQgMtfzyLuCxIOdcme92BMznr07y8aOgxoC7m8QAvD_BwE:G:s&amp;s_kwcid=AL!4422!3!651751060962!e!!g!!amazon%20s3!19852662362!145019251177&amp;gad_campaignid=19852662362&amp;gbraid=0AAAAADjHtp9o0by-fRoumH3v5sj_mXnWa&amp;gclid=CjwKCAiAt8bIBhBpEiwAzH1w6WpItW_9VdmqO8JX-r1G8TQgMtfzyLuCxIOdcme92BMznr07y8aOgxoC7m8QAvD_BwE" rel="noopener noreferrer" target="_blank">Amazon S3</a> with state locking for consistency and collaboration.</p> 
<p><img alt="" class="alignnone size-full wp-image-11940" height="346" src="https://d2908q01vomqb2.cloudfront.net/761f22b2c1593d0bb87e0b606f990ba4974706de/2025/11/17/image-1-15.png" style="margin: 10px 0px 10px 0px;" width="1430" /></p> 
<p><em>Figure 1: Architecture diagram</em></p> 
<h1>Implementation Steps</h1> 
<ol> 
 <li><strong>Develop and commit infrastructure code</strong></li> 
</ol> 
<p>Developers define infrastructure as code (IaC) using Terraform and commit it to a GitHub repository. This enables version control and collaboration while maintaining consistency across environments.</p> 
<ol start="2"> 
 <li><strong>Trigger the automated CI/CD pipeline</strong></li> 
</ol> 
<p>When a change is pushed, a GitHub Actions workflow is triggered. The pipeline connects securely to AWS using OpenID Connect (OIDC), eliminating the need for long-lived credentials.</p> 
<ol start="3"> 
 <li><strong>Run security and compliance checks</strong></li> 
</ol> 
<p>The pipeline performs an automated Bridgecrew (Checkov) scan to validate Terraform code against security and compliance best practices before deployment.</p> 
<ol start="4"> 
 <li><strong>Generate and review the Terraform plan</strong></li> 
</ol> 
<p>The workflow runs Terraform plan to preview infrastructure changes. This step helps teams understand what resources will be created or modified.</p> 
<ol start="5"> 
 <li><strong>Manual approval for deployment</strong></li> 
</ol> 
<p>A manual approval gate ensures that changes are reviewed before applying them, adding governance control to the automation process.</p> 
<ol start="6"> 
 <li><strong>Provision infrastructure on AWS</strong></li> 
</ol> 
<p>Once approved, the pipeline executes terraform apply to deploy the infrastructure. Terraform state files are securely stored in an Amazon S3 bucket, ensuring reliable state management and collaboration.</p> 
<h1>Prepare your AWS environment</h1> 
<p>To begin, make sure you have your AWS multi-account structure and tooling ready:</p> 
<ol> 
 <li>Log in to your AWS management account as a user who has admin access and verify that <a href="https://docs.aws.amazon.com/organizations/latest/userguide/orgs_introduction.html" rel="noopener noreferrer" target="_blank">AWS Organizations</a> is enabled.</li> 
 <li>Create an S3 bucket for Terraform remote state files and an <a href="https://aws.amazon.com/dynamodb/" rel="noopener noreferrer" target="_blank">Amazon DynamoDB</a> table for state locking to prevent concurrent writes.</li> 
 <li>On your local machine (or CI runner), install Terraform (version 1.3 or later), the AWS CLI, Git, and GitHub Actions.</li> 
 <li>Write your Terraform configuration for the landing zone. This will include defining your organization (using the Terraform AWS provider to create OUs and accounts), baseline infrastructure (like an S3 log archive bucket, <a href="http://aws.amazon.com/cloudtrail" rel="noopener noreferrer" target="_blank">AWS CloudTrail </a>for auditing, an<a href="https://aws.amazon.com/config/" rel="noopener noreferrer" target="_blank">d AWS Config </a>rules), and other foundational resources.</li> 
 <li>Create a<a href="https://aws.amazon.com/iam/" rel="noopener noreferrer" target="_blank">n AWS Identity and Access Management </a>(IAM) role (for example, GitHubActionsDeploymentRole) with a trust policy that allows the GitHub Actions OIDC provider to assume it.<br /> a. Attach a policy to this role granting necessary permissions (AWS Organizations, IAM, Amazon S3, and so on, limited to your deployment scope).<br /> b. Note the role Amazon Resource Name (ARN).</li> 
 <li>In your GitHub repository settings, configure an OIDC trust (AWS_PROVIDER with the role ARN) [PL1] or add repository secrets for AWS roles if needed. This will let your GitHub Actions workflow authenticate to AWS securely</li> 
</ol> 
<h1>Configure the CI/CD pipeline</h1> 
<p>Next, set up the GitHub Actions workflow that will deploy the landing zone infrastructure:</p> 
<ol> 
 <li>In your repository, add a workflow YAML file. You can use the following file for example:<a href="https://github.com/ollionorg/aws-landing-zone/blob/main/.github/workflows/workflows.yaml" rel="noopener noreferrer" target="_blank">https://github.com/ollionorg/aws-landing-zone/blob/main/.github/workflows/workflows.yaml</a></li> 
 <li>Add a step in the workflow to run a security scan on the Terraform code before deployment. For instance, use the Bridgecrew Checkov GitHub Action to scan the repository:<br /> name: Checkov IaC Security Scan<br /> uses: bridgecrewio/checkov-action@v12</li> 
 <li>After a successful scan, include a step to run terraform<sub> init</sub> and terraform plan:<br /> name: Terraform Plan<br /> uses: bridgecrewio/checkov-action@</li> 
 <li>Implement a manual approval gate. In GitHub Actions, one approach is to use environment protection rules so that the deploy job waits for approval in the GitHub UI.</li> 
 <li>Add a Terraform apply stage. After the job is approved, the pipeline should perform Terraform apply to execute the changes and create or update the AWS infrastructure.</li> 
</ol> 
<ul> 
 <li></li> 
</ul> 
<h1>Deploy and validate the landing zone</h1> 
<p>Complete the following steps to deploy and validate the landing zone:</p> 
<ol> 
 <li>To start the pipeline, commit and push your changes to the main branch of the repo. It will first perform the Checkov scan, then run the Terraform plan. Make sure the plan outputs the expected creation of OUs, accounts, and other resources.</li> 
 <li>Review the security scan results. If the Checkov scan found any critical security issues (for example, if your Terraform inadvertently tried to create an S3 bucket without encryption), address those findings.</li> 
 <li>When the plan is ready and no blockers are present, proceed with the manual approval.</li> 
 <li>After approval, the Terraform apply will run and provision the infrastructure.</li> 
 <li>Sign in to the <a href="http://aws.amazon.com/console" rel="noopener noreferrer" target="_blank">AWS Management Console </a>and check that everything is set up correctly: 
  <ol type="a"> 
   <li>In AWS Organizations, you should see the new OUs and member accounts created.</li> 
   <li>Check that baseline resources like the log archive S3 bucket, <a href="https://aws.amazon.com/config/" rel="noopener noreferrer" target="_blank">AWS Config</a> recorder, and <a href="https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html" rel="noopener noreferrer" target="_blank">AWS CloudTrail</a> logs are present in the appropriate accounts.</li> 
  </ol> </li> 
</ol> 
<h1>Deploy workloads with a DevOps pipelines (optional)</h1> 
<p>With the landing zone in place, you can use similar pipelines to deploy and manage workloads in your member accounts. This section provides an example of how to deploy an <a href="https://aws.amazon.com/eks/" rel="noopener noreferrer" target="_blank">Amazon Elastic Kubernetes Service </a>(Amazon EKS) cluster using the pipeline module provided:</p> 
<ol> 
 <li>Add the Amazon EKS module configuration.</li> 
 <li>In your Terraform repository (it could be a separate repo or a new directory for workloads), write the Terraform code for the EKS cluster. You can use CloudEQ’s Amazon EKS module, which includes best practice configurations.</li> 
 <li>Add a new GitHub Actions workflow to your repository. This workflow will be similar to the landing zone pipeline. It will assume an IAM role through OIDC, then apply to create the EKS cluster.</li> 
 <li>Commit and push the Amazon EKS module code and workflow. On approval, Terraform will create the EKS cluster and any add-ons defined.</li> 
</ol> 
<p><img alt="GitHub Actions workflow diagram showing EKS cluster setup steps and Terraform operations" class="alignnone size-full wp-image-11936" height="395" src="https://d2908q01vomqb2.cloudfront.net/761f22b2c1593d0bb87e0b606f990ba4974706de/2025/11/17/image-2-3.jpeg" style="margin: 10px 0px 10px 0px;" width="1411" /></p> 
<p><em>Figure 2: Terraform Plan Output</em></p> 
<p><img alt="Security assessment summary showing 37 total findings: 30 low, 7 medium, and zero high to extreme risks" class="alignnone size-full wp-image-11937" height="178" src="https://d2908q01vomqb2.cloudfront.net/761f22b2c1593d0bb87e0b606f990ba4974706de/2025/11/17/image-3-6.jpeg" style="margin: 10px 0px 10px 0px;" width="562" /></p> 
<p><em>Figure 3: Trend Micro vulnerabilities on Terraform Plan</em></p> 
<ol> 
 <li>Verify the new EKS cluster. You can fetch the kubeconfig for the cluster (for example, if output by Terraform) and confirm you can connect.</li> 
 <li>You can go to the Trend Micro console and run your modules with some unique tags to get checks on vulnerabilities, as shown in the following screenshot.</li> 
</ol> 
<p><img alt="Security check results showing 7 filtered checks with 6 successes and 1 failure" class="alignnone size-full wp-image-11938" height="696" src="https://d2908q01vomqb2.cloudfront.net/761f22b2c1593d0bb87e0b606f990ba4974706de/2025/11/17/image-4-5.jpeg" style="margin: 10px 0px 10px 0px;" width="1311" /></p> 
<p><em>Figure 4: Trend Micro vulnerabilities on the Trend-Micro dashboard</em></p> 
<h1>Conclusion</h1> 
<p>CloudEQ’s DevOps pipeline automation with the AWS Well-Architected Framework helps organizations scale on AWS while maintaining governance. This solution can reduce deployment times and includes automated checks to support compliance requirements. The AWS Validated DevOps Pipeline Automation helps organizations align their applications with AWS Well-Architected practices.</p> 
<p>Get started with <a href="https://aws.amazon.com/marketplace/pp/prodview-o2sxtcl5kk4vw?sr=0-1&amp;ref_=beagle&amp;applicationId=AWSMPContessa" rel="noopener noreferrer" target="_blank">CloudEQ DevOps Pipeline Automation</a> in AWS Marketplace.</p> 
<p>To learn more about the solution, contact CloudEQ through the Request private offer option in AWS Marketplace. Our team will discuss your requirements and guide you through implementation.</p> 
<h2>About the authors</h2> 
<p style="clear: both;"><img alt="" class="size-full wp-image-11941 alignleft" height="150" src="https://d2908q01vomqb2.cloudfront.net/761f22b2c1593d0bb87e0b606f990ba4974706de/2025/11/17/dsouza-2-150x150-1.jpeg" width="150" /></p> 
<h3>Ryan Dsouza</h3> 
<p>Ryan Dsouza is a principal solutions architect in the Cloud Optimization organization at Amazon Web Services (AWS). Based in New York City, Ryan helps customers design, develop, and operate more secure, scalable, and innovative solutions using the breadth and depth of AWS capabilities to deliver measurable business outcomes. He is actively engaged in developing strategies, guidance, and tools to help customers architect solutions that optimize for performance, cost-efficiency, security, resilience, and operational excellence, adhering to the AWS Cloud Adoption Framework and AWS Well-Architected Framework</p> 
<p style="clear: both;"><img alt="" class=" size-thumbnail wp-image-11942 alignleft" height="150" src="https://d2908q01vomqb2.cloudfront.net/761f22b2c1593d0bb87e0b606f990ba4974706de/2025/11/17/Priyanka-2025-11-17-120757-150x150.jpg" width="150" /></p> 
<h3>Priyanka Sanjeev</h3> 
<p>Priyanka Sanjeev is a technical program manager in the Cloud Optimization organization at Amazon Web Services (AWS). Based in Seattle, Priyanka spearheaded from concept to deliver the Well-Architected Validated Solutions initiative, in which mechanisms such as automated reviews and remediations and enablement of the Well-Architected Framework were integrated into the solution build and delivery lifecycle. Solutions built following these principles stay Well-Architected through the lifecycle of the workload</p> 
<p style="clear: both;"><img alt="" class="size-thumbnail wp-image-11968 alignleft" height="150" src="https://d2908q01vomqb2.cloudfront.net/761f22b2c1593d0bb87e0b606f990ba4974706de/2025/11/17/Screenshot-2025-11-17-153103-150x150.jpg" width="150" /></p> 
<h3>Kevin Mead</h3> 
<p>Kevin Mead is CloudEQ’s growth architect, with 20 years of experience crafting strategic solutions for Fortune 500 companies. He’s the visionary who identifies opportunities and turns them into business gold. As VP of Business Development, Kevin ensures that CloudEQ’s innovative cloud solutions are tailored to meet each client’s unique needs, driving transformative change and ensuring long-term partnerships. Kevin’s leadership is built on one simple principle: delivering unprecedented value to our clients and partners alike</p>
