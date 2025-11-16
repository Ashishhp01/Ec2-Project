# Ec2-Project
AWS Lambda functions to automatically start and stop EC2 instances based on tags. Instances tagged with Environment=Development and AutoStart=true are started automatically, and instances tagged with AutoStop=true are stopped on schedule. Useful for cost optimization in dev/test environments.
