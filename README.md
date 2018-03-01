# BASH AWS PROFILE

[![screenshot](https://github.com/juhofriman/aws-profile-bash-prompt/raw/master/images/screenshot.png)](#features)

When having more than one AWS account, switching between different accounts can be painfull. This is super simple, yet pretty efficient, way of handling multiple AWS profiles.

With properly configured AWS-CLI, you can execute commands to various AWS accounts using `--profile` switch. I personally consider this **extremely** dangerous approach, because it is really easy to run commands to a completely wrong account. AWS-CLI respects AWS_DEFAULT_PROFILE environment parameter, and one can adjust this by "mutating" bash with `export AWS_DEFAULT_PROFILE=my-profile`. Using sub-shells more safe and easy approach because I want to **see all the time** to which account I'm currently running commands on. That's why we print current AWS_DEFAULT_PROFILE to bash prompt.

So this is simply just opens new bash sub shell with AWS_DEFAULT_PROFILE and PS1 (bash prompt) set nicely. Bash prompt then displays currently active profile, and you will never run commands to wrong account!

This also works great with MFA authentication, which is essential.

Another feature is `aws-sts-temporary-creds-to-env`, which is intended to use in conjunction with `aws-profile`. It simply fetches temporary credentials to new bash subshell, and it is primarily intended to use for testing and trying out AWS-SDK based programs, as they tend to read credentials from environment variables such as `AWS_ACCESS_KEY_ID`.

## Install

Bash AWS Profile is developed with os x, but should work with any bash environment with `sed` available. If you want to use `aws-sts-temporary-creds-to-env` as well, you need [jq](https://stedolan.github.io/jq/).

1. Clone the repo
2. Add `aws-profile` and|or `aws-sts-temporary-creds-to-env` to $PATH as preferred in your environment *(I just simply add whole clone to path)*

## Usage

### aws-profile

Just execute `aws-profile` with profile named in `~/.aws/config` and start running those commands.

```
$ aws-profile
Pass profile name as an argument. Mayhaps one of these?

test
foo
$ aws-profile test
AWS [test]: ~ $
```

Exit with `exit` or ctrl+d.

### aws-sts-temporary-creds-to-env

When you have a session initiated with `aws-profile`, you can execute `aws-sts-temporary-creds-to-env` which opens yet another subshell with temporary credentials in environment.

```
$ aws-profile test
AWS [test]: ~ $ aws-sts-temporary-creds-to-env
AWS [test*]: ~ env | grep AWS_
AWS_SESSION_TOKEN=FQoDY...
AWS_DEFAULT_PROFILE=test
AWS_SECRET_ACCESS_KEY=fwf....
AWS_ACCESS_KEY_ID=AS...
```

See the little star in bash prompt? That means you do have temporary credentials in your shell.

Now you can execute directly simple nodejs program like this without any hassle.

```javascript
const AWS = require("aws-sdk");

const s3 = new AWS.S3({
    apiVersion: '2006-03-01',
    params: {Bucket: 'mybucket'}
});
s3.upload({
    Metadata: {
        "mimeType": 'text/plain'
    },
    ContentType: 'text/plain',
    StorageClass: "REDUCED_REDUNDANCY",
    Key: 'test',
    Body: 'test data'
  });
```

When you are done with temporary credentials, just toss them away with `exit` or ctrl+d. Note that by default, sts temporary credentials are valid for an hour.
