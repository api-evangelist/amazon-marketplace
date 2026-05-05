---
title: "How to reduce AWS Marketplace costs by automating AMI subscription monitoring"
url: "https://aws.amazon.com/blogs/awsmarketplace/how-to-reduce-aws-marketplace-costs-by-automating-ami-subscription-monitoring/"
date: "Thu, 23 Apr 2026 16:30:06 +0000"
author: "Richard Ferraresi"
feed_url: "https://aws.amazon.com/blogs/awsmarketplace/feed/"
---
<p>Managing <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html">Amazon Machine Image (AMI)</a> subscriptions in <a href="https://aws.amazon.com/marketplace/features/what-is-aws-marketplace">AWS Marketplace</a> can be challenging when <a href="https://aws.amazon.com/ec2/">Amazon Elastic Compute Cloud (Amazon EC2)</a> instance types change frequently. Without automation, organizations risk unexpected costs from on-demand licenses when agreements aren’t updated.</p> 
<p>This post shows you how to build an automated AWS Marketplace AMI monitoring solution that tracks EC2 instance type changes and alerts you when subscription amendments are needed. Using <a href="https://aws.amazon.com/eventbridge/">Amazon EventBridge</a>, <a href="https://docs.aws.amazon.com/lambda/">AWS Lambda</a>, and <a href="https://aws.amazon.com/ses/">Amazon Simple Email Service (Amazon SES)</a> in a hub-and-spoke architecture, you can reduce AWS Marketplace costs by up to 72% while eliminating manual monitoring efforts.</p> 
<p>In this post, we show you how to capture instance modifications in real-time, compares them against active subscriptions, and notifies you when action is required—helping you maintain purchased subscriptions that provide significant discounts compared to on-demand pricing. Learn how to deploy this across multiple AWS accounts.</p> 
<h2>Prerequisites for AWS Marketplace AMI subscription monitoring</h2> 
<p>Before you get started, make sure you have the following prerequisites:</p> 
<ol> 
 <li>An <a href="https://aws.amazon.com/account/">AWS account</a></li> 
 <li><a href="https://aws.amazon.com/organizations/">AWS Organizations</a> enabled</li> 
 <li><strong>Basic familiarity with AWS services</strong> including AWS Lambda, Amazon EventBridge, and Amazon EC2</li> 
</ol> 
<h2>AWS Marketplace AMI monitoring solution overview</h2> 
<p>This solution automates AMI subscription monitoring by capturing EC2 instance type changes and triggering notifications when amendments are needed.</p> 
<p><strong>How it works</strong>:</p> 
<p>When you stop an EC2 instance, Lambda records its current state (instance type and AMI license) into <a href="https://aws.amazon.com/dynamodb/">Amazon DynamoDB</a>. When you modify the instance type, EventBridge captures the change, Lambda compares the new configuration against the stored state, and if an amendment is needed, sends a notification via Amazon SES with instance details and a direct link to amend the agreement.</p> 
<p>The hub account components:</p> 
<ol> 
 <li>EventBridge custom event bus with resource policies</li> 
 <li>Lambda function for state management and change detection</li> 
 <li>DynamoDB table for instance state persistence</li> 
 <li>Amazon SES for notifications</li> 
</ol> 
<p>The following architecture diagram shows the integration between EventBridge, Lambda, DynamoDB, and Amazon SES.</p> 
<div class="wp-caption alignleft" id="attachment_12099" style="width: 1041px;">
 <img alt="Figure 1: AWS Marketplace AMI monitoring hub account architecture diagram" class="wp-image-12099 size-full" height="649" src="https://d2908q01vomqb2.cloudfront.net/761f22b2c1593d0bb87e0b606f990ba4974706de/2026/04/21/AWS_Marketplace_part_1.jpg" width="1031" />
 <p class="wp-caption-text" id="caption-attachment-12099">Figure 1: AWS Marketplace AMI monitoring hub account architecture diagram</p>
</div> 
<p>Spoke accounts require only Amazon EventBridge rules that forward Amazon EC2 lifecycle events to the hub account’s event bus. This design centralizes monitoring logic and supports scaling across multiple accounts. The following diagram illustrates this architecture.</p> 
<div class="wp-caption alignleft" id="attachment_12100" style="width: 1041px;">
 <img alt="Figure 2: AWS Marketplace AMI monitoring spoke account architecture with Amazon EventBridge rules forwarding to hub account" class="size-full wp-image-12100" height="649" src="https://d2908q01vomqb2.cloudfront.net/761f22b2c1593d0bb87e0b606f990ba4974706de/2026/04/21/AWS_Marketplace_part_2.jpg" width="1031" />
 <p class="wp-caption-text" id="caption-attachment-12100">Figure 2: AWS Marketplace AMI monitoring spoke account architecture with Amazon EventBridge rules forwarding to hub account</p>
</div> 
<p><strong>Note</strong>: You can configure the Lambda function with a <code>SkipAgreementVerification</code> parameter. When set to <strong>true</strong>, the solution sends notifications for all EC2 instance type changes on instances with AWS Marketplace AMI annual subscriptions, enabling comprehensive monitoring across all instances.</p> 
<h2>Step-by-step guide: Deploy AWS Marketplace AMI monitoring solution</h2> 
<p>The solution uses <a href="https://aws.amazon.com/cloudformation/">AWS CloudFormation</a> <a href="https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/introduction.html">templates</a> to deploy the hub-and-spoke architecture. You’ll deploy the hub account resources first, then the Amazon SES email validation, and finally deploy the spoke account resources in each AWS account you want to monitor.</p> 
<p>To deploy the hub account resources, follow these steps:</p> 
<ol> 
 <li>Download CloudFormation templates <a href="https://github.com/aws-samples/sample-marketplace-ami-amendment-monitor/blob/main/hub-account-template.yaml">hub-account-template.yaml</a> and <a href="https://github.com/aws-samples/sample-marketplace-ami-amendment-monitor/blob/main/spoke-account-template.yaml">spoke-account-template.yaml</a> from the <a href="https://github.com/aws-samples/sample-marketplace-ami-amendment-monitor">GitHub repository</a>.</li> 
 <li>Sign in to the <a href="https://console.aws.amazon.com/">AWS Management Console</a> and open the CloudFormation console in your hub account.</li> 
 <li>Choose <strong>Create stack</strong>, then choose <strong>With new resources (standard)</strong>, as shown in the following screenshot.</li> 
</ol> 
<div class="wp-caption alignleft" id="attachment_12102" style="width: 988px;">
 <img alt="Figure 3: AWS CloudFormation console create stack interface for AWS Marketplace monitoring deployment" class="size-full wp-image-12102" height="660" src="https://d2908q01vomqb2.cloudfront.net/761f22b2c1593d0bb87e0b606f990ba4974706de/2026/04/21/Figure3_CreateStack-1.png" width="978" />
 <p class="wp-caption-text" id="caption-attachment-12102">Figure 3: AWS CloudFormation console create stack interface for AWS Marketplace monitoring deployment</p>
</div> 
<ol start="4"> 
 <li>Under <strong>Specify template</strong>, select <strong>Upload a template file</strong>.</li> 
 <li>Choose <strong>Choose file</strong> and upload the <code>hub-account-template.yaml</code> Then choose <strong>Next</strong>.</li> 
 <li>On the <strong>Specify stack detail</strong> page, enter a stack name (for example, <code>ec2-monitoring-hub</code>). This name serves as the prefix for all created resources.</li> 
 <li>Configure the following parameters:</li> 
</ol> 
<ol> 
 <li> 
  <ul> 
   <li><strong>EmailFrom:</strong> The sender email address for alert notifications</li> 
   <li><strong>EmailRecipient:</strong> Email address(es) to receive notifications (separate multiple emails by comma)</li> 
   <li><strong>EnableEmailNotifications: </strong>When set to true, sends email notifications. When set to false, email notifications are disabled.</li> 
   <li><strong>SkipAgreementVerification:</strong> When set to true, the solution tracks all EC2 instance type changes for instances launched from AWS Marketplace AMIs, regardless of whether an active subscription exists. Leave this set to false (default) to only track instances with active annual AMI subscriptions.</li> 
  </ul> </li> 
</ol> 
<ol start="8"> 
 <li>Choose <strong>Next</strong>.</li> 
 <li>On the <strong>Configure stack options</strong> page, configure any additional options as needed, then choose <strong>Next</strong>. Under <strong>Capabilities</strong>, select the acknowledgment checkbox because the solution creates <a href="https://aws.amazon.com/iam/">AWS Identity and Access Management (IAM)</a> Then choose <strong>Next</strong>.</li> 
 <li>On the Review page, review your configurations and choose <strong>Submit</strong>.</li> 
</ol> 
<p><strong>Note</strong>: AWS CloudFormation will automatically create the Amazon SES email identity for the sender address (EmailFrom parameter). You must verify this email address before the solution can send notifications.</p> 
<p>Repeat the following steps for each email address that should receive notifications:</p> 
<ol> 
 <li>Sign in to the AWS Management Console and navigate to the Amazon Simple Email Service console in your hub account.</li> 
 <li>Choose <strong>Identities</strong>, then choose <strong>Create identity</strong></li> 
 <li>Under <strong>Identity details,</strong> choose the <strong>Email address</strong></li> 
 <li>Enter the email address you specified in the <strong>EmailRecipient </strong>parameter described in the previous step. Then choose <strong>Create identity</strong></li> 
</ol> 
<div class="wp-caption alignleft" id="attachment_12103" style="width: 1234px;">
 <img alt="Figure 4: Amazon SES Create Identity" class="size-full wp-image-12103" height="862" src="https://d2908q01vomqb2.cloudfront.net/761f22b2c1593d0bb87e0b606f990ba4974706de/2026/04/21/Image4_CreateIdentity.png" width="1224" />
 <p class="wp-caption-text" id="caption-attachment-12103">Figure 4: Amazon SES Create Identity</p>
</div> 
<ol start="5"> 
 <li>To complete verification, click the link in the confirmation email you receive.</li> 
</ol> 
<p>To deploy the spoke accounts resources, follow these steps:</p> 
<p>Repeat the following steps in each AWS account where you want to monitor EC2 instance type changes for AMI subscriptions.</p> 
<ol> 
 <li>Sign in to the AWS Management Console and navigate to the AWS CloudFormation console in your spoke account.</li> 
 <li>Choose <strong>Create stack</strong>, then choose <strong>With new resources (standard)</strong>.</li> 
 <li>Under <strong>Specify template</strong>, select <strong>Upload a template file</strong>.</li> 
 <li>Choose <strong>Choose file</strong> and upload the <code>spoke-account-template.yaml</code> Then choose <strong>Next</strong>.</li> 
 <li>On the <strong>Specify stack details</strong> page, enter a stack name (for example, <code>ec2-monitoring-spoke</code>).</li> 
 <li>Configure the following parameters:</li> 
</ol> 
<ol> 
 <li> 
  <ol> 
   <li><strong>HubAccountId: </strong>The AWS account ID where you deployed the hub account stack (from the previous procedure).</li> 
   <li><strong>HubEventBusName:</strong> The name of the EventBridge event bus in the hub account. This is automatically generated by the hub stack as <code>&lt;HubStackName&gt;-Bus</code> (for example, if your hub stack is named <code>ec2-monitoring-hub</code>, the event bus name is <code>ec2-monitoring-hub-Bus</code>).</li> 
   <li><strong>HubRegion:</strong> The <a href="https://docs.aws.amazon.com/glossary/latest/reference/glos-chap.html#region">AWS Region</a> where you deployed the hub account stack</li> 
  </ol> </li> 
</ol> 
<ol start="7"> 
 <li>Choose <strong>Next</strong>.</li> 
 <li>On the <strong>Configure stack options</strong> page, configure any additional options as needed, then choose <strong>Next</strong>. Under <strong>Capabilities</strong>, select the acknowledgment checkbox because the solution creates IAM resources. Then choose <strong>Next</strong>.</li> 
 <li>On the Review page, review your configurations and choose <strong>Submit</strong>.</li> 
</ol> 
<p>After completing these steps, your solution is ready to monitor EC2 instance type changes and send notifications when AMI subscription amendments are needed.</p> 
<h2>How to test your AWS Marketplace AMI monitoring setup</h2> 
<p>To verify the solution is working correctly, follow these steps to simulate an EC2 instance type change:</p> 
<ol> 
 <li>Identify a test Amazon EC2 instance that was launched from an AWS Marketplace AMI with an active subscription.</li> 
 <li>On the <a href="https://console.aws.amazon.com/ec2/">Amazon EC2 console</a>, select the instance and choose <strong>Instance state</strong>, then <strong>Stop instance</strong>.</li> 
 <li>Wait for the instance to reach the stopped state.</li> 
 <li>With the instance still selected, choose <strong>Actions</strong>, then <strong>Instance settings</strong>, then <strong>Change instance type</strong>.</li> 
 <li>Select a different instance type from the dropdown menu and choose <strong>Apply</strong>.</li> 
 <li>Within 5–10 minutes, you should receive an email notification at the address you configured during deployment.</li> 
</ol> 
<p>The email will include details about the instance change, including the instance ID, previous instance type, new instance type, and the associated AMI subscription that might require amendment alongside the link to amend it.</p> 
<h2>Key benefits of automated AWS Marketplace AMI monitoring</h2> 
<p>This automated monitoring solution delivers three key advantages:</p> 
<ol> 
 <li><strong>Cost control</strong> – Automated alerts provide visibility into when amendments may be needed, helping you maintain purchased subscriptions that provide discounts compared to On-Demand pricing</li> 
 <li><strong>Risk mitigation</strong> – Because AWS Marketplace amendments are contract modifications involving financial decisions, the solution notifies you rather than automatically amending agreements, providing human oversight for subscription changes</li> 
 <li><strong>Reduced manual effort</strong> – Manually tracking whether EC2 instance type changes require agreement amendments is error-prone and time-consuming. This solution eliminates the need for constant manual monitoring</li> 
</ol> 
<p>With this approach, you can maintain control over your AWS Marketplace AMI subscriptions while maximizing the value of your license investments.</p> 
<h2>Cleanup</h2> 
<p>To avoid charges to your account, delete the CloudFormation stack and resources. For more information, see <a href="https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-delete-stack.html">Deleting a stack on the AWS CloudFormation console in the AWS CloudFormation User Guide</a>.</p> 
<h2>Conclusion</h2> 
<p>This automated monitoring solution delivers the three key benefits outlined above: cost control, reduced manual effort, and maintained oversight. Download the CloudFormation templates from the GitHub repository to get started and share your feedback with your AWS account team.</p> 
<h2>About the authors</h2> 
<h3><strong><img alt="Richard Ferraresi" class="size-thumbnail wp-image-12084 alignleft" height="150" src="https://d2908q01vomqb2.cloudfront.net/761f22b2c1593d0bb87e0b606f990ba4974706de/2026/04/21/fsrich_final.jpg" width="150" />Richard Ferraresi</strong></h3> 
<p>Richard Ferraresi is a Senior Technical Account Manager at Amazon Web Services (AWS), where he helps large enterprise customers optimize their cloud infrastructure. With a passion for making technology accessible to all, Richard focuses on innovative solutions that drive sustainable growth and measurable business outcomes. Outside of work, he enjoys playing tennis and chess, watching cult classic films, and exploring business strategy and sales methodologies.</p> 
<h3><strong><img alt="author" class="size-thumbnail wp-image-12084 alignleft" height="150" src="https://d2908q01vomqb2.cloudfront.net/761f22b2c1593d0bb87e0b606f990ba4974706de/2026/04/21/luduar_final.jpg" width="150" />Luis Duarte</strong></h3> 
<p>Luis Duarte is a Senior Solutions Architect at Amazon Web Services (AWS), where he helps customers design secure, resilient, and cost-effective solutions. He combines deep technical expertise with business knowledge, holding an MBA and a master’s degree in business innovation, alongside his software engineering background. When not helping energy customers innovate with AI and cloud technologies, Luis enjoys exploring the intersection of technology and business strategy using domain-driven design.</p>
