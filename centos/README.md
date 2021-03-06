# CentOS 7 AMI for AWS

It's first try to use Packer build AMI, which focused on CentOS 7. Packer is powerful tool and could do far more than here.

## Prerequisites

1. Install Packer [here][e4cf82c3].
2. Install VirtualBox [here][480dd043].
3. (Option) Download http://mirror.vodien.com/centos/7.6.1810/isos/x86_64/CentOS-7-x86_64-DVD-1810.iso into local disk and modify "iso_file", or replace "iso_file" to that link in centos7_template.json.

  [e4cf82c3]: https://www.packer.io/intro/getting-started/install.html "here"
  [480dd043]: https://www.virtualbox.org/ "here"

## Environments

```
AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_DEFAULT_REGION=
S3_BUCKET_NAME=bucket-4-packer
```
## Create S3 Bucket
```
aws s3api create-bucket --bucket bucket-4-packer
```

## Create IAM Role & Policy
```
aws iam create-role --role-name vmimport --assume-role-policy-document file://trust-policy.json

aws iam put-role-policy --role-name vmimport --policy-name vmimport --policy-document file://role-policy.json
```


## Build OVA & AMI
```
#Be patient!!! > 20 minutes
packer build centos7_template.json
```


## Try different ISO

1. Modify variables->iso_file in centos7_template.json
2. Adjust variables->iso_checksum in centos7_template.json

On Mac OS X,
```
iso_fileshasum -a 256 CentOS-????????.iso
```

On Linux,
```
sha256sum CentOS-????????.iso
```

## References
https://github.com/boxcutter/centos

http://work.haufegroup.io/automate-ami-with-packer/

https://www.packer.io/docs/post-processors/amazon-import.html
