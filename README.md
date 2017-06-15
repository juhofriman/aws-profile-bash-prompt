# BASH AWS PROFILE

Simple bash utility for configuring new sub shell with AWS_DEFAULT_PROFILE.

I just hate adding --profile to aws-cli. This opens new subshell with AWS_DEFAULT_PROFILE configured and displays neat prompt.

## Usage

```
bash:~ fooz$ aws s3 ls

An error occurred (AccessDenied) when calling the ListBuckets operation: Access Denied
bash:~ fooz$ aws-profile personal
AWS [personal]: aws s3 ls
2016-12-09 13:31:36 my-foo-bar
AWS [personal]: exit
exit
```

Works well with MFA authentication as well.
