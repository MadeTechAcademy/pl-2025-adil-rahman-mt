# aws access

## Tasks

Knowing what you already know about connecting to the aws sandbox and running command line arguments

- review anything in the tips documentation you want to brush up on
- connect to the aws sandbox in a terminal window
- run the aws command line command to retrieve a list of s3 buckets on the account

## Notes

AWS vault allows us to securely store and manage AWS credentials and use them to generate temporary credentials for AWS commands which reduces the risk of leaking credentials.

To use AWS vault, we use `aws-vault exec <profile> -- <command>` e.g. `aws-vault exec mt-playground -- aws s3 ls` will list all AWS S3 buckets.
