# lambda-precompile-package
A set of CloudFormation stacks that will create the necessary infrastructure to compile Python packages on Amazon Linux.

## Steps
1) Launch bootstrap stack to create IAM resources and S3 bucket.
2) Sync templates to the bucket:
* aws s3 sync aws-cloudformation/arch-yamls s3://<devops_bucket_name>/lambda-precompile-package/templates
3) Create an EC2 keypair named <environment>-precompile-key in your region of choice
4) Launch the landing_zone template, filling in the parameters(all of which are required).
5) Wait for all of the child stacks to come up - when complete, you'll be able to log into the instance.