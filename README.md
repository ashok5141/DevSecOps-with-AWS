# DevSecOps-with-AWS
Implementing DevSecOps with AWS CodeCommit and Pipeline Using Snyk, SonarCloud, and OWASP ZAP

DevSecOps integrates security practices into the DevOps process, ensuring continuous security throughout the software development lifecycle. This overview details the implementation of a DevSecOps pipeline using AWS CodeCommit, AWS CodePipeline, Snyk, SonarCloud, and OWASP ZAP.

#AWS Components and Tools
AWS CodeCommit: A fully managed source control service that hosts secure Git-based repositories.
AWS CodePipeline: A continuous integration and continuous delivery (CI/CD) service for fast and reliable application and infrastructure updates.
Snyk: A developer-first security platform that automatically finds and fixes vulnerabilities in code and open-source dependencies.
SonarCloud: A cloud-based code quality and security service that identifies bugs, vulnerabilities, and code smells in code.
OWASP ZAP: An open-source web application security scanner to find vulnerabilities in web applications.
Pipeline Setup
1. Setting Up AWS CodeCommit
Create a Repository: Set up a repository in AWS CodeCommit to store your source code.
Clone Repository: Clone the repository to your local machine and add your project files.
2. Configuring AWS CodePipeline
Create a Pipeline: In AWS CodePipeline, create a new pipeline.
Source Stage: Configure the source stage to pull code from your AWS CodeCommit repository.
Build Stage: Set up a build stage using AWS CodeBuild or a similar build tool. This stage compiles your code and runs initial unit tests.
3. Integrating Snyk
Snyk CLI: Add Snyk CLI commands to your buildspec file in AWS CodeBuild. This allows Snyk to scan for vulnerabilities in your dependencies and code.
Snyk Integration: Integrate Snyk into your build process to automatically test for and fix security vulnerabilities.
4. Integrating SonarCloud
SonarCloud Account: Create an account on SonarCloud and set up a new project.
SonarCloud Scanner: Add the SonarCloud scanner to your buildspec file. Configure it with the necessary credentials and project key.
Quality Gate: Configure SonarCloud quality gates to ensure that the code meets your security and quality standards before moving to the next pipeline stage.
5. Integrating OWASP ZAP
OWASP ZAP Docker: Use the OWASP ZAP Docker container in your buildspec file to scan your web application for security vulnerabilities.
Automated Scans: Configure OWASP ZAP to perform automated scans during the pipeline execution.
Report Generation: Generate and review security reports from OWASP ZAP to identify and fix vulnerabilities.

#Benefits
Continuous Security: Automated security checks are integrated into every stage of the pipeline.
Early Detection: Vulnerabilities and code quality issues are identified early in the development process.
Compliance: Ensures code complies with industry security standards and best practices.
Efficiency: Streamlines the development and security processes, reducing manual intervention.

# IAM

Sonar Cloud - (OrganizationName-JavaVulnerableSite, ProjectName-AshokDemoRepo, ) free plan logged in with GitHub

Sonal CLoud - token (awstoken_javavulnerablesite)

AWS Secrets Manager - Secret Name:Secret Key - FirstSecretSonarCloud:FirstSecretSonar

### Quality Gate in SonarCloud

**Quality Gate failed code build also fails, avoid any kind production issue**

Quality gates are a key feature of SonarCloud (and SonarQube) that provide a way to enforce code quality standards and ensure that the code meets certain criteria before it is considered ready for production. They define a set of conditions that the code must meet, such as code coverage, duplication, complexity, and the absence of critical bugs or vulnerabilities.

In the context of integrating SonarCloud with AWS CodeBuild or other CI/CD tools, quality gates help automate the process of code quality checks and provide immediate feedback to developers.

**QualityGateRule-**You can create your custom rule in Sonar cloud or Use exit rule by making it as **default**

## AWS CodePipeline

AWS CodePipeline is a continuous integration and continuous delivery (CI/CD) service for fast and reliable application and infrastructure updates. CodePipeline automates the build, test, and deploy phases of your release process every time there is a code change, based on the release model you define. This allows you to rapidly and reliably deliver features and updates.

### How AWS CodePipeline Works

1. **Source Stage**: Retrieves the source code from a source repository (e.g., AWS CodeCommit, GitHub, Bitbucket).
2. **Build Stage**: Compiles the source code, runs unit tests, and packages the code using a build service (e.g., AWS CodeBuild).
3. **Test Stage**: Runs tests (e.g., Unit, integration, Performance, security tests) to validate the build. Tools JUnit, Selenium, or Custom Scripts for different types of testing.
4. **Deploy Stage**: Deploys the application to various environments (e.g., staging, production) using deployment services (e.g., AWS CodeDeploy, AWS Elastic Beanstalk).
5. **Approval Stage**: Allows for manual approval before progressing to the next stage.

### AWS CodePipeline in DevSecOps

DevSecOps integrates security practices into the DevOps process, ensuring that security is an integral part of the CI/CD pipeline. AWS CodePipeline can be used to implement DevSecOps practices by incorporating security checks and audits at various stages of the pipeline.

### Key Practices for Using AWS CodePipeline in DevSecOps

1. **Integrate Static Code Analysis**:
    - Use tools like SonarQube or Amazon CodeGuru Reviewer to perform static code analysis during the build stage.
    - Example: Configure a CodeBuild project in the build stage that runs static analysis tools.
2. **Run Security Tests**:
    - Incorporate security testing tools such as OWASP ZAP, Snyk, or Checkmarx to perform vulnerability scanning during the test stage.
    - Example: Add a CodeBuild project in the test stage that runs security tests and reports any vulnerabilities.
3. **Automated Compliance Checks**:
    - Implement automated compliance checks using tools like AWS Config, AWS Security Hub, and custom scripts to ensure adherence to security policies.
    - Example: Add a stage that triggers a Lambda function to run compliance checks.
4. **Manual Approval for Critical Deployments**:
    - Implement manual approval stages to review and approve changes before deploying to production.
    - Example: Add an approval stage before the final deploy stage to ensure human oversight.
5. **Use Infrastructure as Code (IaC)**:
    - Manage infrastructure using IaC tools like AWS CloudFormation, Terraform, or AWS CDK to ensure consistent and repeatable infrastructure deployment.
    - Example: Add a build stage that validates CloudFormation templates or Terraform scripts for security best practices.
    
    ### Snyk
    
    Snyk is an organization that develops security tools (SaaS) to secure:
    
    - Source Code
    - Open Source/Third Part libraries
    - Container (Docker, Kubernetes)
    - Infrastructure as Code (we can use Terraform scripts for IAC)
    
    Insert Snyk in in between - **Source → Snyk → Build**
    
    ### OWASP ZAP
    
    OWASP ZAP is an open-source web application security scanner. It is intended to be used by both those new to application security as well as professional penetration testers. It is one of the most active Open Web Application Security Projects and has given Flagship status. It can scan both:
    
    - Web Application
    - API Specifications
    - Mobile Application
    
    ### Amazon S3
    
    **Amazon Simple Storage Service (S3)** is an object storage service that offers industry-leading scalability, data availability, security, and performance. S3 buckets are containers for storing objects (files). Each bucket can store an unlimited number of objects and can be managed through the AWS Management Console, AWS CLI, SDKs, and REST API.
    **The OWASP ZAP scan results will stored on AWS S3 bucket**
    
    In AWS Code build → Edit → Artifacts → Select S3 buckets (It will automatically generate name of S3 bucket).
    
    | Feature | Source Code Analysis (SAST) | Software Composition Analysis (SCA) |
    | --- | --- | --- |
    | Focus | Internal, custom-written code | External, open-source components and dependencies |
    | Analysis Method | Scans code for vulnerabilities, coding errors, and security weaknesses | Identifies open-source components, analyzes for known vulnerabilities and license compliance |
    | Requires Source Code | Yes | May not (can analyze compiled code or binaries in some cases) |
    | Tools | * Fortify  * CodeSonar  * Coverity | * Snyk  * WhiteSource  * Nexus Lifecycle |
    | Example | SAST tool identifies a potential buffer overflow vulnerability in a custom login function | SCA tool identifies a known security vulnerability (Heartbleed) within the OpenSSL library used by the application |
    
    ## Project Requirement
    
    | Development | Building and Running Tests | Reporting |
    | --- | --- | --- |
    | Java Code created by developer | Maven - Build Tool | Identifying false positives |
    |  | Sonar Cloud-SAST |  |
    |  | SNYK-SCA |  |
    |  | OWASP ZAP -DAST |  |
    
    **End to End  AWS DevSecOps Pipeline (SAST, SCA & DAST) for JAVA Project:**
    
    1. Update buildspec.yml
    2. Update pom.xml
    
    my SNYK token - 4496cf1c-abb9-4e55-bb56-0e769a2e625f
    
    SonarCloud-Jira Integration:f047e34edb9894bb3e4ef2eac5febfa760da51d7
    
    Integrating SonarCloud with with JIRA dashboard
    
    ![JIRA_Sonarcloud_Integration_Dashboard.png](https://raw.githubusercontent.com/ashok5141/DevSecOps-with-AWS/refs/heads/main/Security_Reports/JIRA_Sonarcloud_Integration_Dashboard.png)
    
    # AWS Security Services
    
    ### AWS Security Hub
    
    **AWS Security Hub** is a comprehensive security service that provides a unified view of your security alerts and compliance status across your AWS environment. It collects, aggregates, and prioritizes security findings from multiple AWS services, partner solutions, and your own custom security products. 
    
    **Key Features and Capabilities**
    
    1. **Centralized View**: Provides a single place to view and manage security alerts and compliance status across AWS accounts.
    2. **Automated Compliance Checks**: Continuously runs automated security checks based on industry standards and best practices, such as CIS AWS Foundations Benchmark.
    3. **Aggregation of Security Findings**: Collects and consolidates security findings from various AWS services and third-party solutions.
    4. **Insights**: Uses built-in and customizable insights to analyze and prioritize findings.
    5. **Integration with AWS Services and Partner Solutions**: Integrates seamlessly with AWS services and third-party security tools for comprehensive security management.
    6. **Remediation Guidance**: Offers actionable remediation guidance to help address security issues. 
    
    **Sub-Services and Integrations**
    
    AWS Security Hub integrates with several AWS services and third-party tools to enhance its capabilities:
    
    1. **AWS Services**:
        - **Amazon GuardDuty**: Provides intelligent threat detection.
        - **AWS Config**: Monitors and evaluates AWS resource configurations.
        - **Amazon Macie**: Finds and protects sensitive data.
        - **AWS Firewall Manager**: Manages firewall rules across multiple accounts.
        - **AWS Systems Manager**: Provides operational insights and remediation actions.
        - **Amazon Inspector**: Assesses application security and compliance.
    2. **Third-Party Integrations**:
        - Security Hub integrates with various third-party security solutions like CrowdStrike, Palo Alto Networks, Splunk, Sumo Logic, and others for extended visibility and threat detection capabilities.
        
        ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/18e45da0-b084-4b4d-a8b7-a50446324a3a/5dca3cfb-badb-4ed5-a6b9-c1bb5716d780/Untitled.png)
        
        Before starting AWS Security hub need to enable the AWS config service. 
        
        **AWS Config:** Records and evaluates configurations of your AWS resources, A summarized view of AWS and non-AWS resources and the compliance status of the rules and resources in each AWS Region.
        
        **AWS Config has 154 (toady 6 July 2024) managed rules for AWS config if any of the rules violated it notify as security concern.**
        
        ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/18e45da0-b084-4b4d-a8b7-a50446324a3a/8c2f1a12-15ec-4cfc-904b-4204ba944970/Untitled.png)
        
        After starting AWS config, back to the **AWS Security Hub,** here we have 
        
        **Security Standards**
        
        - [x]  Enable AWS Foundational Security Best Practice v1.0.0
        - [ ]  Enable AWS Resource Tagging Standard v1.0.0
        - [x]  Enable CIS AWS Foundations Benchmark v1.2.0
        - [ ]  Enable CIS AWS Foundations Benchmark v1.4.0
        - [ ]  Enable CIS AWS Foundations Benchmark v3.0.0
        - [x]  Enable NIST Special Publication 800-53 Revision 5
        - [ ]  Enable PCI DSS v3.2.1
        
        I enable above 3 security standards for AWS resources.
        
    
    ### AWS Inspector
    
    Is a automated and continual vulnerability management at scale, That continual scans workloads for software vulnerabilities and unintended network exposure.
    
    - Automated discovery and continual scanning
    - Single pane of glass for all vulnerabilities
    - Highly contextualized risk score for prioritization
    - Automate vulnerability management workloads
    
    Where as **AWS Guard Duty** check all resources of AWS logs then notify to the user.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/18e45da0-b084-4b4d-a8b7-a50446324a3a/d56c70c9-727c-4a5e-a013-722788251c59/Untitled.png)
    
    
    
    
    
    # Terraform
    
    Downloads terraform for windows. Copy the path in environmental variables,
    
    You can run any path from windows

   # Coming Soon
   Kubernetes Security, More Projects with DevSecOps.
