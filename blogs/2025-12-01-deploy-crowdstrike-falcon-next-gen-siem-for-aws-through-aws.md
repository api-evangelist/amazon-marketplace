---
title: "Deploy CrowdStrike Falcon Next-Gen SIEM for AWS through AWS Marketplace"
url: "https://aws.amazon.com/blogs/awsmarketplace/deploy-crowdstrike-falcon-next-gen-siem-aws-through-aws-marketplace/"
date: "Mon, 01 Dec 2025 00:00:39 +0000"
author: "Jenn Reed"
feed_url: "https://aws.amazon.com/blogs/awsmarketplace/feed/"
---
<p><a href="https://aws.amazon.com/marketplace/pp/prodview-vubjuepxztndi">CrowdStrike Falcon for AWS</a> in <a href="https://aws.amazon.com/marketplace/">AWS Marketplace</a> is a pay-as-you-go offering AWS customers can use to help protect their cloud workloads using the CrowdStrike Falcon platform and only pay for what they use. The Falcon platform on <a href="https://aws.amazon.com/">Amazon Web Services (AWS)</a> is a unified security platform for enterprise-grade security solutions at scale. This offering includes security information event management (SIEM) and cloud security modules, CrowdStrike Falcon Next-Gen SIEM and CrowdStrike Falcon Cloud Security. Falcon Next-Gen SIEM includes a new automation experience that simplifies the onboarding of the complex configurations of <a href="https://aws.amazon.com/organizations/">AWS Organizations</a> to provide visibility and security monitoring, analysis, detection, and response all within one platform. It does this by using <a href="https://aws.amazon.com/iam/">AWS Identity and Access Management (IAM)</a> cross-account read-only asset discovery roles using <a href="https://aws.amazon.com/cloudformation/">AWS CloudFormation</a>. In addition to IAM, AWS Marketplace deploys the Falcon Next-Gen SIEM connectors for <a href="https://aws.amazon.com/cloudtrail/">AWS CloudTrail</a>, <a href="https://aws.amazon.com/guardduty/">Amazon GuardDuty</a> and <a href="https://aws.amazon.com/security-hub/">AWS Security Hub</a>.</p> 
<p>In this post, we show you how to use the automation experience in AWS Marketplace to deploy Falcon Next-Gen SIEM for AWS across all AWS Accounts in your AWS Organization. We then demonstrate how to connect AWS CloudTrail, AWS Security Hub, and Amazon GuardDuty.</p> 
<h2>Solution overview</h2> 
<p>CrowdStrike and AWS have created an enhanced version of <a href="https://aws.amazon.com/about-aws/whats-new/2023/11/saas-quick-launch-aws-marketplace/">SaaS Quick Launch</a> for Falcon Next-Gen SIEM in AWS Marketplace, delivering a streamlined deployment experience so customers can quickly deploy and access <a href="https://www.crowdstrike.com/en-us/platform/next-gen-siem/">Falcon Next-Gen SIEM for AWS </a>in minutes.</p> 
<h3>CrowdStrike Falcon Next-Gen SIEM for AWS architecture</h3> 
<p>Falcon Next-Gen SIEM is a security software-as-a-service (SaaS) hosted on AWS. It uses AWS services running in a customer’s AWS accounts to deploy customer data connectors using <a href="https://aws.amazon.com/eventbridge/">Amazon EventBridge</a>, <a href="https://aws.amazon.com/sns/">Amazon Simple Notification Service (Amazon SNS)</a>, and <a href="https://aws.amazon.com/sqs/">Amazon Simple Queue Service (Amazon SQS)</a> to send AWS event and security data to Falcon Next-Gen SIEM. The customer’s Falcon Next-Gen SIEM infrastructure is fully managed by CrowdStrike using IAM using cross-account roles and AWS CloudFormation.</p> 
<p>The following diagram shows the solution architecture.</p> 
<p><img alt="CrowdStrike Next-Gen SIEM Architecture Diagram" class="wp-image-11991 size-full" height="849" src="https://d2908q01vomqb2.cloudfront.net/761f22b2c1593d0bb87e0b606f990ba4974706de/2025/11/23/CrowdStrikeMarketplaceArch20251028.png" width="1745" /></p> 
<p style="text-align: center;"><em>Figure 1: CrowdStrike Falcon Next-Gen SIEM for AWS architecture</em></p> 
<h2>Solution walkthrough: Deploy CrowdStrike Next-Gen SIEM for AWS through AWS Marketplace</h2> 
<p>In the following steps, we show you how to subscribe to CrowdStrike Falcon for AWS in AWS Marketplace. We then use the new launch experience to deploy Falcon Next-Gen SIEM. The solution follows a two-step process:</p> 
<ol> 
 <li>Start your CrowdStrike Falcon for AWS subscription</li> 
 <li>Deploy CrowdStrike Falcon Next-Gen SIEM for AWS</li> 
</ol> 
<h3><strong>Start your CrowdStrike Falcon for AWS subscription</strong></h3> 
<p>Follow these steps to subscribe to CrowdStrike Falcon for AWS in AWS Marketplace:</p> 
<ol> 
 <li>In your AWS management account, open the <a href="https://aws.amazon.com/marketplace/pp/prodview-vubjuepxztndi">CrowdStrike Falcon for AWS</a> product detail page and choose <strong>View purchase options</strong>.</li> 
 <li>Choose <strong>Subscribe</strong>.</li> 
 <li>Your subscription might take a couple minutes to process. In the meantime, to begin the deployment integration process, click <strong>Set up your account</strong> (Figure 2).</li> 
 <li>If you receive a dialog box to Enable AWS Marketplace deployment integration, choose <strong>Enable and continue</strong>.</li> 
</ol> 
<p><img alt="Set up Your Account" class="wp-image-11994 " height="83" src="https://d2908q01vomqb2.cloudfront.net/761f22b2c1593d0bb87e0b606f990ba4974706de/2025/11/23/SetupYourAccount-scaled.jpg" width="762" /></p> 
<p style="text-align: center;"><em>Figure 2: </em><em>Set up your account redirect</em></p> 
<h3><strong>Deploy CrowdStrike Falcon Next-Gen SIEM for AWS</strong></h3> 
<p>You will be taken to the new streamlined experience that will guide you through CrowdStrike authentication, Falcon Next-Gen SIEM for AWS configuration, and launch. Follow these steps:</p> 
<ol> 
 <li>You will be redirected to the CrowdStrike account registration page. Follow the on-screen prompts to register with CrowdStrike. This can take 15 minutes for activation. Wait until you receive the account activation email before you proceed to the next step. .</li> 
 <li>Return to AWS Marketplace and notice the success message indicating that your CrowdStrike account has been linked, as shown in the following screenshot. Choose <strong>Next</strong>.</li> 
</ol> 
<p><img alt="CrowdStrike Account Linking Successful" class="aligncenter wp-image-11995 " height="138" src="https://d2908q01vomqb2.cloudfront.net/761f22b2c1593d0bb87e0b606f990ba4974706de/2025/11/23/CrowdStrikeAccountLinkinSuccess-scaled.jpg" width="759" /></p> 
<p style="text-align: center;"><em>Figure 3: Cr</em><em>owdStrike account linking confirmation message</em></p> 
<ol start="3"> 
 <li>In the <strong>Configure deployment R</strong><strong>and access role</strong> section, keep the default parameters. Choose <strong>Next</strong>.</li> 
 <li>In the <strong>Configure AWS CloudTrail i</strong> section, it will have selected the location where your organizational AWS CloudTrail for management events is configured. Keep the default parameters. Choose <strong>Next</strong>.</li> 
 <li>In the <strong>Configure AWS Security Hub integration</strong> section, it will have selected the AWS account and home Region where either AWS Security Hub <a href="https://aws.amazon.com/what-is/cspm/">cloud security posture management (CSPM)</a> or AWS Security Hub is configured. It will then create an Amazon EventBridge rule to send AWS Security Hub events to the CrowdStrike Amazon EventBridge event-bus for Falcon Next-Gen SIEM. Keep the default. Choose <strong>Next</strong>.</li> 
 <li>In the <strong>Configure Amazon GuardDuty</strong> <strong>integration</strong> section, it will have selected the AWS account and Regions where Amazon GuardDuty is configured. It will then create an Amazon EventBridge rule to send Amazon GuardDuty events to the CrowdStrike Amazon EventBridge event-bus for Falcon Next-Gen SIEM. Keep the default parameters. Choose <strong>Next</strong>.</li> 
 <li>In the <strong>Review and launch</strong> section, choose <strong>Deploy resources</strong>. During the next few minutes, the application integration and identity resources necessary to deploy Falcon Next-Gen SIEM, will be installed across all AWS accounts in your AWS Organization. Follow the on-screen prompts to access your new Falcon Next-Gen SIEM quick start connectors page, as shown in the following screenshot.</li> 
</ol> 
<p><img alt="CrowdStrike Falcon Next-Gen SIEM quick start connectors page" class="aligncenter wp-image-11996 " height="357" src="https://d2908q01vomqb2.cloudfront.net/761f22b2c1593d0bb87e0b606f990ba4974706de/2025/11/23/2a-Quickstart-AWS-CloudTrail.png" width="634" /></p> 
<p style="text-align: center;"><em>Figure 4: CrowdStrike Falcon Next-Gen SIEM quick start connectors page</em></p> 
<h2><strong>Conclusion</strong></h2> 
<p>In this post, we demonstrated how to subscribe to and use CrowdStrike Next-Gen SIEM for AWS available in AWS Marketplace. For more information, visit <a href="http://go.crowdstrike.com/crowdstrike-and-aws/">CrowdStrike Falcon for AWS</a>.</p> 
<h2><strong>About Authors</strong></h2> 
<footer> 
 <div class="blog-author-box"> 
  <div class="blog-author-image">
   <img alt="Jenn Reed" class="alignleft wp-image-11636 size-full" height="180" src="https://d2908q01vomqb2.cloudfront.net/761f22b2c1593d0bb87e0b606f990ba4974706de/2025/11/21/Jenn.png" width="130" />
  </div> 
  <h3 class="lb-h4">Jenn Reed</h3> 
  <p>Jenn Reed is a Global Principal Security Solutions Architect at AWS with over 25 years of deep experience working in cyber security and software development. She is based out of Ann Arbor MI. At AWS, she is focused on helping customers build securely with AWS.</p> 
 </div> 
 <div class="blog-author-box"> 
  <div class="blog-author-image">
   <img alt="Kunjal Botadra " class="alignleft wp-image-11636 size-full" height="180" src="https://d2908q01vomqb2.cloudfront.net/761f22b2c1593d0bb87e0b606f990ba4974706de/2025/11/21/kb.jpg" width="130" />
  </div> 
  <h3 class="lb-h4">Kunjal Botadra</h3> 
  <p>Kunjal Botadra is a Senior Product Manager at Amazon Web Services (AWS), focusing on software delivery and procurement solutions. He drives the strategy and roadmap for enterprise software deployment. Previously at Akamai Technologies, Kunjal developed web performance optimization products and services. He specializes in customer-centric product development and building high-performing cross-functional teams.</p> 
 </div> 
</footer>
