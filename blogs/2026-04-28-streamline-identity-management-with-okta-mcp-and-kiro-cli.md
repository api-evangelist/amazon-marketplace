---
title: "Streamline identity management with Okta MCP and Kiro CLI"
url: "https://aws.amazon.com/blogs/awsmarketplace/streamline-identity-management-with-okta-mcp-and-kiro-cli/"
date: "Tue, 28 Apr 2026 19:30:46 +0000"
author: "Sunil Ramachandra"
feed_url: "https://aws.amazon.com/blogs/awsmarketplace/feed/"
---
<p>Identity and access management (IAM) platforms generate large volumes of operational data such as user profiles, group memberships, access logs, and authentication events. Administrators typically navigate this data through multiple consoles and dashboards. This manual approach slows onboarding, makes access reviews time-consuming, and complicates troubleshooting permissions and meeting compliance requirements.</p> 
<p>AI capabilities in identity workflows can help improve how development and security teams interact with this data. Rather than manually querying systems and interpreting raw outputs, teams can automatically surface user data, analyze access patterns at scale, and accelerate identity operations end to end, shifting from reactive console navigation to proactive, intelligent automation.</p> 
<p><a href="https://www.okta.com/">Okta</a> is an identity and access management platform that provides secure authentication and authorization for applications and users.</p> 
<p>The <a href="https://github.com/okta/okta-mcp-server">Okta Model Context Protocol (MCP)</a> server enables AI agents to interact with your Okta organization through natural language, providing access to core identity operations including:</p> 
<ul> 
 <li>Querying user information and status</li> 
 <li>Analyzing group memberships</li> 
 <li>Reviewing access patterns</li> 
 <li>Troubleshooting authentication issues</li> 
 <li>Monitoring identity posture and entitlements</li> 
</ul> 
<p><a href="https://kiro.dev/">Kiro</a> is an agentic, AI-powered <a href="https://aws.amazon.com/what-is/ide/">integrated development environment (IDE)</a> built by <a href="https://aws.amazon.com/">Amazon Web Services (AWS)</a>, designed to take developers from prototype to production. <a href="https://kiro.dev/cli/">Kiro command line interface (Kiro CLI)</a> is an AI-powered command line tool that brings Kiro’s agentic development capabilities directly to your terminal. Using CLI commands to access the MCP server, you can automate complex workflows into existing DevOps pipelines. It provides a scriptable interface that eliminates the overhead of a graphical user interface (GUI), enabling scaling and control over environment configurations.</p> 
<p>The Okta MCP server integrates with Kiro CLI, providing contextual identity assistance within your development environment.</p> 
<p>This post walks you through deploying the Okta MCP server and integrating it with Kiro to help manage identity operations through natural language commands. Instead of navigating multiple consoles or writing complex queries, you can retrieve user data, analyze access patterns, and perform identity operations with natural language commands.</p> 
<h2>Why subscribe to Okta through AWS Marketplace</h2> 
<p>When you subscribe to <a href="https://aws.amazon.com/marketplace/pp/prodview-r4vzqg4bgndda">Okta through AWS Marketplace</a>, you gain access to a tool designed to help simplify procurement, reduce contracting overhead by using your existing AWS agreements, and consolidate billing—reducing time spent on procurement processes.</p> 
<p>By procuring Okta Platform through <a href="https://aws.amazon.com/marketplace/">AWS Marketplace</a>, you can simplify both procurement and billing while using the same identity capabilities built with enterprise security considerations.</p> 
<h2>Prerequisites</h2> 
<p>Before deploying the Okta MCP server with Kiro CLI, you need to set up your Okta application, install the required tools, clone the Okta MCP server repository, set up Kiro and your development environment. To complete these prerequisites, follow the steps in the next sections.</p> 
<p>Set up your Okta application:</p> 
<ol> 
 <li>Subscribe to Okta Platform in AWS Marketplace.</li> 
 <li>Sign in to your Okta admin console and navigate to <strong>Applications</strong> and then <strong>Applications</strong>.</li> 
 <li>Choose <strong>Create App Integration</strong> and select <strong>native application</strong> as the application type.</li> 
 <li>Enable the <a href="https://developer.okta.com/docs/guides/device-authorization-grant/main/">Device Authorization Grant</a> flow for CLI authentication.</li> 
 <li>Grant the necessary <a href="https://developer.okta.com/docs/api/oauth2">OAuth scopes</a>: <code>`okta.users.read`, `okta.logs.read`, `okta.groups.read`</code>.</li> 
 <li>Copy the generated <strong>Client ID</strong>. You need it during the MCP server configuration.</li> 
 <li>Copy your Okta org URL. You need it during the MCP server configuration.</li> 
</ol> 
<p><img alt="Screenshot of the Okta application console showing the app configuration page. The client ID is highlighted in the center of the screen, and the Okta org URL is visible in the top-right corner of the browser." class="aligncenter size-full wp-image-12136" height="328" src="https://d2908q01vomqb2.cloudfront.net/761f22b2c1593d0bb87e0b606f990ba4974706de/2026/04/22/okta.png" width="864" /></p> 
<p style="text-align: center;"><strong><em>Figure 1: Okta application console to configure your app</em></strong></p> 
<p>For detailed information about creating applications, refer to the <a href="https://developer.okta.com/docs/guides/create-an-app-integration/saml2/main/">Okta documentation</a>.</p> 
<p>Install these required tools on your system:</p> 
<ul> 
 <li>Python 3.8 or later</li> 
 <li><code>`uv`</code> package manager for Python dependency management</li> 
</ul> 
<p>Setup the Okta MCP server repository:</p> 
<p>Install <a href="https://github.com/okta/okta-mcp-server">Okta MCP Server</a> that provides integration with Okta’s Admin Management APIs. It allows LLM agents to interact with Okta in a programmatic way, enabling automation and enhanced management capabilities.</p> 
<p>Set up Kiro and your development environment:</p> 
<p><a href="https://kiro.dev/docs/enterprise/subscribe/">Subscribe to Kiro</a> and <a href="https://kiro.dev/docs/cli/installation/">configure Kiro CLI</a> with your AWS account.</p> 
<h2>Solution overview: Streamline identity management with Okta MCP and Kiro CLI</h2> 
<p>This solution deploys Okta’s MCP server that bridges AI-powered IDEs such as Kiro with your Okta organization, creating an API connection designed to help AI agents execute identity operations through natural language interactions.</p> 
<p>The MCP server exposes Okta’s capabilities, including user management, group membership, and access membership, and is designed to support your team’s security and compliance efforts through the OAuth-based Device Authorization Grant flow.</p> 
<h3>Create the MCP server configuration file</h3> 
<p>To configure MCP server access for Kiro, create or edit the global configuration file at ~/.kiro/settings/mcp.json or create a workspace-specific configuration at .kiro/settings/mcp.json, replacing the placeholder values as follows:</p> 
<ul> 
 <li><code><em>`&lt;path/to/okta-mcp-server&gt;`</em></code>: Full path to your cloned okta-mcp-server directory</li> 
 <li><code><em>`&lt;your-org.okta.com&gt;`</em></code>: Your Okta organization URL</li> 
 <li><code><em>`&lt;your-client-id&gt;`</em></code>: The client ID from your application</li> 
</ul> 
<p><code>{</code><br /> <code>&nbsp; "mcpServers": {</code><br /> <code>&nbsp;&nbsp;&nbsp; "okta-mcp-server": {</code><br /> <code>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "command": "uv",</code><br /> <code>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "args": [</code><br /> <code>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "run",</code><br /> <code>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "--directory",</code><br /> <code>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "/path/to/okta-mcp-server",</code><br /> <code>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "okta-mcp-server"</code><br /> <code>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ],</code><br /> <code>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "env": {</code><br /> <code>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "OKTA_ORG_URL": "https://your-org.okta.com",</code><br /> <code>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "OKTA_CLIENT_ID": "your-client-id",</code><br /> <code>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "OKTA_SCOPES": "okta.users.read okta.groups.read okta.logs.read"</code><br /> <code>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; }</code><br /> <code>&nbsp;&nbsp;&nbsp; }</code><br /> <code>&nbsp; }</code><br /> <code>}</code></p> 
<h3>Verify the configuration</h3> 
<p>To test the integration, open a terminal and run:</p> 
<p><code>kiro-cli</code></p> 
<p>The following screenshot shows the Kiro terminal.</p> 
<p><img alt="Screenshot of the Kiro IDE showing the Terminal tab with the Kiro CLI running. The terminal displays the okta-mcp-server being initialized and ready for use." class="aligncenter size-full wp-image-12134" height="1608" src="https://d2908q01vomqb2.cloudfront.net/761f22b2c1593d0bb87e0b606f990ba4974706de/2026/04/22/Screenshot-2026-04-22-at-3.25.37 PM.png" width="3350" /></p> 
<p style="text-align: center;"><strong><em>Figure 2: Kiro IDE with Kiro CLI terminal with </em>okta-mcp-server<em> initialized</em></strong></p> 
<p>On first launch, okta-mcp-server loads and you’re prompted to authenticate using the Device Authorization Grant flow. Follow the on-screen instructions to complete authentication.</p> 
<p>The following screenshot shows the Okta authentication screen for activating your device.</p> 
<p><img alt="Screenshot of the browser authentication prompt for the device authorization grant flow, where the user confirms sign-in by verifying the device code displayed on screen." class="aligncenter size-full wp-image-12130" height="478" src="https://d2908q01vomqb2.cloudfront.net/761f22b2c1593d0bb87e0b606f990ba4974706de/2026/04/22/Picture4-4.png" width="864" /></p> 
<p style="text-align: center;"><strong><em>Figure 3: Browser authentication for device authorization grant</em></strong></p> 
<h3>Test the MCP server connection</h3> 
<p>After authentication is complete, in the KIRO CLI Terminal try asking:</p> 
<p><code>list users</code></p> 
<p>You should see the MCP server load successfully and return user information from your Okta organization.</p> 
<h2>Use cases</h2> 
<p>In this section, we present three examples of using natural language to interact with your Okta organization.</p> 
<h3>User onboarding verification</h3> 
<p>Ask Kiro:</p> 
<p><code>Show me all users created in the last 30 days in Okta</code>.</p> 
<p>Kiro CLI connects to the Okta MCP server and orchestrates the query automatically. It interprets your natural language request, understands you want recently created Okta users, and calculates the 30-day date range back from the current date. In this case, the start date of the period is February 9, 2026. The Okta MCP server, installed and configured locally as a <a href="https://modelcontextprotocol.io/docs/getting-started/intro">Model Context Protocol</a> integration, exposes tools such as list_users, get_user, create_user, and others that map directly to Okta’s API operations. Kiro selects list_users because it supports search filters and calls it through the Okta MCP server, which authenticates and queries Okta on your behalf.</p> 
<p>The MCP server returns the matching user profiles through the protocol, and Kiro CLI formats the raw JSON response into a clean summary table for you.</p> 
<p>The following screenshot shows the resulting table in Kiro.</p> 
<p><img alt="Screenshot of the Kiro interface, showing Kiro running the list_users tool and the resulting table with columns for name, email or login, and user ID. The table lists two users." class="aligncenter size-full wp-image-12128" height="1120" src="https://d2908q01vomqb2.cloudfront.net/761f22b2c1593d0bb87e0b606f990ba4974706de/2026/04/22/Screenshot-2026-04-22-at-3.08.41 PM.png" width="1850" /></p> 
<p style="text-align: center;"><strong><em>Figure 4: Kiro produces a result in a tabular format for users created in the last 30 days</em></strong></p> 
<h3>Troubleshooting provisioning failures</h3> 
<p>Ask Kiro:</p> 
<p><code>Why did test@example.com user's provisioning fail?</code></p> 
<p>Kiro CLI connects to the Okta MCP server and orchestrates the diagnosis. It searches Okta logs using get_logs, discovers duplicate identities using list_users, and retrieves the full user profile using get_user, all routed through the MCP server. The investigation reveals repeated application.provision.user.sync failures to <a href="https://aws.amazon.com/iam/identity-center/">AWS IAM Identity Center</a> on March 9, pointing to a likely System for Cross-domain Identity Management (SCIM) conflict, expired bearer token, or userName attribute mismatch between Okta and AWS. Next, it provides the recommendations for fixing the issue.</p> 
<p>The following screenshot shows Kiro troubleshooting the provisioning failures.</p> 
<p><img alt="Screenshot of the Kiro console identifying fields and values of application.provision.user.sync failures and listing three most likely causes and recommended next steps." class="aligncenter size-full wp-image-12126" height="1386" src="https://d2908q01vomqb2.cloudfront.net/761f22b2c1593d0bb87e0b606f990ba4974706de/2026/04/22/Screenshot-2026-04-22-at-3.03.47 PM.png" width="2398" /></p> 
<p style="text-align: center;"><strong><em>Figure 5: Kiro troubleshooting a provisioning and troubleshooting</em></strong></p> 
<h3>Bulk entitlement report</h3> 
<p>Ask Kiro:</p> 
<p><code>Provide a breakdown of all Okta users, including what access and permissions each user has been granted on their respective AWS accounts</code></p> 
<p>Kiro CLI pulls all users from Okta using the Okta MCP server list_users, checks IAM Identity Center for matching identities, queries AWS account assignments, resolves permission set names, extracts <a href="https://aws.amazon.com/iam/">AWS Identity and Access Management (IAM)</a> policies, and generates a consolidated entitlement report of IAM roles and permission sets.</p> 
<p>The report flags un-provisioned users and overprivileged access across your AWS environment.</p> 
<p><img alt="Screenshot of the AWS Entitlement Report showing a user access matrix with columns for user, login, AWS synced, account access, permission set, IAM policy, and status." class="aligncenter size-full wp-image-12138" height="790" src="https://d2908q01vomqb2.cloudfront.net/761f22b2c1593d0bb87e0b606f990ba4974706de/2026/04/22/Screenshot-2026-04-22-at-4.11.34 PM.png" width="2798" /></p> 
<p style="text-align: center;"><em><strong>Figure 6: Kiro generates Okta – AWS Entitlement Report</strong> </em></p> 
<h2>Clean up</h2> 
<p>To remove the integration, delete the MCP server configuration from mcp.json and restart Kiro CLI. Optionally, deactivate the Okta application in the Okta administrator console.</p> 
<h2>Conclusion</h2> 
<p>By deploying the Okta MCP server with Kiro CLI, you can manage identity workflows through conversational commands—querying user data, analyzing access patterns, and simplifying operations without switching tools.</p> 
<h2>Get started today</h2> 
<p>To get started, subscribe to <a href="https://aws.amazon.com/marketplace/pp/prodview-r4vzqg4bgndda">Okta Platform in AWS Marketplace</a> for consolidated billing, simplified procurement, and direct integration with Kiro CLI. Deploy the Okta MCP server and experience conversational identity management in your development environment.</p> 
<h2><strong>About Authors</strong></h2> 
<footer> 
 <div class="blog-author-box"> 
  <div class="blog-author-image">
   <img alt="" class="aligncenter size-full wp-image-11636" height="220" src="https://d2908q01vomqb2.cloudfront.net/761f22b2c1593d0bb87e0b606f990ba4974706de/2026/04/23/Screenshot-2026-04-23-at-10.10.33 AM.png" width="170" />
  </div> 
  <h3 class="lb-h4">Sunil Ramachandra</h3> 
  <p>Sunil Ramachandra is a Senior Solutions Architect enabling hyper-growth SaaS ISVs to innovate on Amazon Web Services (AWS). He partners with customers to build highly scalable and resilient cloud architectures and is an active builder in the generative AI space, developing agentic AI tools and workflows that help customers and field teams move faster.Connect with <a href="https://www.linkedin.com/in/suramac/">Sunil on LinkedIn</a></p> 
 </div> 
 <div class="blog-author-box"> 
  <div class="blog-author-image">
   <img alt="" class="aligncenter size-full wp-image-11636" height="200" src="https://d2908q01vomqb2.cloudfront.net/761f22b2c1593d0bb87e0b606f990ba4974706de/2026/04/23/Screenshot-2026-04-23-at-10.14.42 AM.png" width="150" />
  </div> 
  <h3 class="lb-h4">Nehal Sangoi</h3> 
  <p>Nehal Sangoi is a Senior Technical Account Manager at Amazon Web Services (AWS). She provides strategic technical guidance to help independent software vendors plan and build solutions using AWS best practices. Connect with <a href="https://www.linkedin.com/in/nehalsangoi/">Nehal on LinkedIn</a></p> 
 </div> 
 <div class="blog-author-box"> 
  <div class="blog-author-image">
   <img alt="" class="aligncenter size-full wp-image-11636" height="200" src="https://d2908q01vomqb2.cloudfront.net/761f22b2c1593d0bb87e0b606f990ba4974706de/2026/04/23/Screenshot-2026-04-23-at-10.16.34 AM.png" width="150" />
  </div> 
  <h3 class="lb-h4">Kapil Patil</h3> 
  <p>Kapil Patil is a Senior Partner Technical Architect at Okta and is responsible for identifying and defining integrations between the Okta and identity-centric ISV products. Kapil comes from an implementation consulting background with deep knowledge of integration, development, and architecting technology solutions. Connect with <a href="https://www.linkedin.com/in/kapil-patil-cloudarchitect/">Kapil on LinkedIn</a>.</p> 
 </div> 
</footer>
