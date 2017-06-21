# BASH AWS PROFILE

When having more than one AWS account, switching between different accounts can be painfull. This is super simple, yet pretty efficient, way of handling multiple AWS profiles.

To my experience, people usually simply add `--profile` switch to all aws-cli commands. I consider this **extremely** dangerous approach, because it is really easy to run commands to completely wrong account. Aws-cli respects AWS_DEFAULT_PROFILE environment parameter, and one can adjust this by "mutating" bash with `export AWS_DEFAULT_PROFILE=my-profile`. I consider using sub-shells more safe and easy approach because I want to **see all the time** to which account I'm currently running commands on.

This also works great with MFA authentication, which is essential.

## Install

Bash AWS Profile is developed with os x, but should work with any bash environment with `sed` available.

1. Clone the repo
2. Add `aws-profile` to $PATH as preferred in your environment *(I use wrapper ~/bin/aws-profile which points to my clone)*

## Usage

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
