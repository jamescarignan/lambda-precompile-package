# lambda-precompile-package
A set of CloudFormation stacks that will create the necessary infrastructure to compile Python packages on Amazon Linux. Assumes no existing infrastructure(i.e. particular VPCs or subnets etc), instead it provisions everything necessary for it to run. You only pay for its lifetime costs(a cent or fraction thereof per run, as it's ~10m).

## Steps
1) Launch bootstrap stack to create IAM resources and S3 bucket.
2) Sync templates to the bucket:
* aws s3 sync aws-cloudformation/arch-yamls s3://<devops_bucket_name>/lambda-precompile-package/templates
3) Create an EC2 keypair named <environment>-precompile-key in your region of choice
4) Launch the landing_zone template, filling in the parameters(all of which are required).
5) Wait for all of the child stacks to come up - when complete, you'll be able to log into the instance.
6) Of course, you don't even need to do this - your package and its dependencies have been zipped and uploaded to s3://<devops_bucket_name>/lambda-precompile-package/packages/<package_name>.zip!
7) You'll receive an email with a link to the file(which you'll need to download with **aws s3 cp**, as it isn't publicly accessible)
8) Should you want to compile some more while you're at it, though, you can do so with:
* pip3 install <package_name> -t .
You'll still need to zip it and transfer it off the instance though.
9) Once finished, terminate the parent stack if you don't want to keep it around(and pay for it).