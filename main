#!/bin/bash

#########################
# Author: Abdullah
# Date: 8th march
#
# Version: v1
#
# after writing this script set the permissions by chmod (4,2,1)
# This script will sum up and report the AWS resource usage
#########################

set -x
set -e
set -o

# AWS S3
# AWS EC2
# AWS Lambda 
# AWS IAM users

# list s3 buckets
echo "Print list of s3 buckets"
aws s3 ls

# list ec2 instances
echo "Print list of ec2 instances"
aws ec2 describe-instances | jq '.Reservations[].Instances[].instanceid'

# list lambda
echo "Print list of lambda functions"
aws lambda list-functions

#list iam users
echo "Print list of iam users"
aws iam list-users

:wq! 
