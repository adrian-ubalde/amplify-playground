# amplify-playground

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
