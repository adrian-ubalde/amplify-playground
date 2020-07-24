# Amplify Playground

A repo for AWS Amplify Console experiments.

## Instructions

https://dev.to/rhymu8354/git-renaming-the-master-branch-137b

https://aws.amazon.com/blogs/mobile/deploy-a-vuejs-app-with-aws-amplify-console-using-cloudformation/

aws cloudformation deploy \
  --template-file ./cloudformation.yaml \
  --capabilities CAPABILITY_IAM \
  --parameter-overrides \
      OauthToken=<GITHUB PERSONAL ACCESS TOKEN> \
      Repository=https://github.com/adrian-ubalde/amplify-playground \
      Domain=thesmartsuperlist.com \
  --stack-name TodoApp


## Outstanding Questions

How do I stop a push of a CloudFormation template change or README update from triggering a deployment? Would disabling master or other branch auto-builds help?  
    Alternatives
        1) Separate repos: 1) HTML/JS/CSS files only Git-repo (no readme); 2) CloudFormation repo
        2) Single repo - multiple GitHub builds using ignore/include paths (https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#onpushpull_requestpaths) E.g 1 build for deploying CloudFormation; 1 build for starting Amplify Console Deployment
        3) To skip an automatic build on a particular commit, include the text [skip-cd] at the end of the commit message. Can a developer consistently do this correctly? What if a push includes multiple commits, each with/without the [skip-cd] message? https://docs.aws.amazon.com/amplify/latest/userguide/build-settings.html#skip-build-for-a-commit
        
How do I prevent CloudFormation update bugs from affecting production website?
    Alternatives:
        1) Separate AWS accounts
        2) Separate CF-stacks - preprod, prod (and dev)
How do I enable pull-request deployments?

## Decision Log

Amplify Console
* How to authorize Amplify to my Github repos via code? N/A - manual step wasn't required when deploying Amplify App via Amplify Console/CloudFormation+AWS-CLI

I chose to use Amplify Console to host my website because it automatically takes care of hosted-zone managing and enabling HTTPS (which CloudFormation doesn't offer out of the box). 

I chose to use Amplify Console to host my website because it offers low latency via CloudFront CDN out of the box.

I chose to use Amplify Console to host my website because it offers website previews centered around Git-repo branches which is feature that is nice to have.

I chose to use Amplify Console as a Continous Deployment tools because it saves me to from having write my own CD pipeline and it's Git-repo branch focus is a simple, easy to understand way to deployment websites with no downtime/auto-rollback.

Downsides I may encounter in the future for chosing Amplify Console is that I lost visibility into the CloudFront distribution behind the service. I see some configuration settings for the distribution via the Amplify Console API but not as complete as the CloudFront API itself. Same may go for the S3 bucket(s) behind Amplify Console.

I chose Amplify Console over Amplify CLI because I think a Continuous Deployment pipeline written declaratively is easier to understand than an imperative approach.

What features does CF lack that I need for deploying the Static front-end site?
* DNS-validated ACM public cerficate for CloudFront distribution
    - Use CDK DnsValidatedCertificate construct?
        * Con: It's an experimental API
    - Use Amplify?
        - Via console, you can point your Amplify project to your R53-hosted-zone
            which will automatically create a wildcard public cert (in ACM?)
        - Via Cloudformation - https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-amplify-domain.html
* Deployment of static files
    - Use CDK S3-deployment construct?
        * Con: It's an experimental API
        * Pros:
            * Simple - only one lambda
    - Use Amplify Console? https://docs.amplify.aws/cli/hosting
        * Cons:
            - AWS charges $0.01 per build minute.
            - AWS charges for traffic-served (does this apply to custom domains as well?)
            - AWS charges for storage/GB-stored (which is affected by number of builds) - $0.023 per GB.
                How to clean up space? How to clean up old builds or PRs builds?
        * Pros:
            - Amplify is supported by CloudFormation.
            - Amplify Console supports Continuous deployment
            - Amplify Console supports instant cache invalidation on each Git commit. https://aws.amazon.com/blogs/mobile/aws-amplify-console-supports-instant-cache-invalidation-and-delta-deployments/
            - Amplify Console supports atomic deployments (i.e. failed updates don't affect existing deployment).
            - Amplify let's you set password access per branch:
            - Amplify let's you view Github PRs in console and view them live (e.g. https://pull-request-1.example.com)

What features does CF lack that I need for deploying the .Net backend API?

## Miscellaneous Personal Notes
https://docs.aws.amazon.com/amplify/latest/userguide/build-settings.html
You can edit these settings in the Amplify Console by choosing App settings>Build settings. These build settings are applied to all the branches in your app, except for the branches that have a YML file saved in the repository.

Feature Branch Deployments
https://docs.aws.amazon.com/amplify/latest/userguide/multi-environments.html

Does AWS Amplify Console support private Git servers?
We currently do not support private Git servers.

What Git source code providers does AWS Amplify Console support?
You can connect private and public repositories from GitHub, BitBucket, GitLab, and AWS CodeCommit.
