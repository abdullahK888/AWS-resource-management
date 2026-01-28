# AWS Resource Management ![ai-gen](https://img.shields.io/badge/ai--gen-ready-blue)

Summarize and report AWS resources in the current account/region. The repository contains a single script (main.bash) that lists S3 buckets, EC2 instances, Lambda functions, and IAM users.

---

## Table of Contents

- [Requirements](#requirements)
- [Permissions](#permissions)
- [Installation](#installation)
- [Usage](#usage)
- [Example output](#example-output)
- [Notes & Fixes](#notes--fixes)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

---

## Requirements

- Unix-like environment with Bash
- AWS CLI (v1 or v2) configured (aws configure or AWS_* env vars)
- jq (optional, for JSON parsing)
- Read/list IAM permissions (see below)

---

## Permissions

Grant least-privilege read access for:

- s3:ListAllMyBuckets
- ec2:DescribeInstances
- lambda:ListFunctions
- iam:ListUsers

Adjust policies to restrict resources or regions as needed.

---

## Installation

Clone the repo and make the script executable:

```bash
git clone https://github.com/abdullahK888/AWS-resource-management.git
cd AWS-resource-management
chmod +x main.bash
```

No further installation required.

---

## Usage

Run from the repository directory:

```bash
./main.bash
# or
bash main.bash
```

Ensure the AWS CLI is authenticated to the intended account and region (AWS_DEFAULT_REGION or --region).

---

## Example output

Typical sections printed by the script (actual content depends on your account):

- S3 buckets (aws s3 ls)
- EC2 instance IDs
- Lambda functions (JSON)
- IAM users (JSON)

Illustrative snippet:

```
Print list of s3 buckets
2023-01-12 15:00:00 example-bucket

Print list of ec2 instances (IDs)
i-0abcd1234efgh5678

Print list of lambda functions
{ "Functions": [ { "FunctionName": "my-function", ... } ] }

Print list of iam users
{ "Users": [ { "UserName": "alice", ... } ] }
```

---

## Notes & Fixes

The included main.bash is a quick utility but contains a few issues. Key fixes:

- Use safer shell options:
  - Replace `set -x; set -e; set -o` with:
    ```bash
    set -euo pipefail
    # use set -x for debugging if needed
    ```
- EC2 Instance ID parsing: the field is `InstanceId` (capital I).
  - Prefer AWS CLI query:
    ```bash
    aws ec2 describe-instances --query 'Reservations[].Instances[].InstanceId' --output text
    ```
  - Or with jq:
    ```bash
    aws ec2 describe-instances | jq -r '.Reservations[].Instances[].InstanceId'
    ```
- Remove stray editor artifacts (e.g., `:wq!`) and ensure jq is installed if you rely on it.
- Consider adding `--region` option or respecting AWS_DEFAULT_REGION, and handle pagination if needed.

Minimal corrected script example:

```bash
#!/usr/bin/env bash
set -euo pipefail

echo "Print list of s3 buckets"
aws s3 ls

echo "Print list of ec2 instances (IDs)"
aws ec2 describe-instances --query 'Reservations[].Instances[].InstanceId' --output text

echo "Print list of lambda functions"
aws lambda list-functions

echo "Print list of iam users"
aws iam list-users
```

---

## Troubleshooting

- "aws: command not found" — Install AWS CLI and ensure it's in PATH.
- "Unable to locate credentials" — Run `aws configure` or set AWS_* env vars.
- JSON parsing failures — Install jq or use AWS CLI `--query`/`--output` flags.
- Permission errors — Verify IAM policy actions listed above.
- Script errors or stray editor text — Clean the script of artifacts and ensure it is executable.

---

## Contributing

Contributions welcome:
- Fix script issues (flags, parsing, region handling)
- Add options (region, output format)
- Add more AWS services or output formats (table/JSON)

Please open issues or pull requests on GitHub.

---

## License

No license file is included. Add a LICENSE file to specify terms before publishing publicly.