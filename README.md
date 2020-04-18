# get-account-authorization-details

This is a boto3 script that runs the AWS IAM [get-account-authorization-details](https://docs.aws.amazon.com/cli/latest/reference/iam/get-account-authorization-details.html) command easily and stores the results in `account-alias.json`.

There are a few nifty features built in:
* You can specify `--profile all` to authenticate to AWS and run this command on every profile specified in your AWS credentials file.
* Avoids rate limiting issues with AWS.

I'm mostly publishing this because I've seen a few questions about proper boto3 usage of this API call recently. It is easier to just point people to this repository so they can use it as a reference.

## Prerequisites

Install Python dependencies. Choose your poison.

* Venv

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

* Pipenv 

```bash
pipenv shell
pipenv install
./get-account-authorization-details.py
```

## Usage

```
Usage: get-account-authorization-details.py [OPTIONS]

  Runs aws iam get-authorization-details on all accounts specified in the
  aws credentials file, and stores them in account-alias.json

Options:
  --profile TEXT                  Specify 'all' to authenticate to AWS and
                                  analyze *all* existing IAM policies. Specify
                                  a non-default profile here. Defaults to the
                                  'default' profile.  [required]
  --output PATH                   Path to store the output. Defaults to
                                  current directory.
  --credentials-file PATH         Path to the AWS credentials file.
  --include-non-default-policy-versions
                                  When downloading AWS managed policy
                                  documents, also include the non-default
                                  policy versions. Note that this will
                                  dramatically increase the size of the
                                  downloaded file.
  --include-unattached            Include unattached IAM policies as well as
                                  attached policies.
  -v, --verbosity LVL             Either CRITICAL, ERROR, WARNING, INFO or
                                  DEBUG
  --help                          Show this message and exit.
```

Commands:

```bash
# Basic usage
get-account-authorization-details.py --profile 4thalulz
# Download everything.
get-account-authorization-details.py --include-non-default-policy-versions --include-unattached
```

## Notes
* By default, it does not download unattached policies. If you want them, you can specify the `--include-unattached` flag.
* By default, it does not include the non-default policy versions for space reasons. Otherwise, you'll run into some size and time issues. For example, downloading the 70+ versions of the `ReadOnlyAccess` policy will take up 10,000 lines in a 30,000 line JSON file. But if you really want it, you can specify the `--include-non-default-policy-versions` flag.
