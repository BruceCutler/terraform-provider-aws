---
subcategory: "S3 (Simple Storage)"
layout: "aws"
page_title: "AWS: aws_s3_bucket_ownership_controls"
description: |-
  Manages S3 Bucket Ownership Controls.
---

# Resource: aws_s3_bucket_ownership_controls

Provides a resource to manage S3 Bucket Ownership Controls. For more information, see the [S3 Developer Guide](https://docs.aws.amazon.com/AmazonS3/latest/dev/about-object-ownership.html).

## Example Usage

```terraform
resource "aws_s3_bucket" "example" {
  bucket = "example"
}

resource "aws_s3_bucket_ownership_controls" "example" {
  bucket = aws_s3_bucket.example.id

  rule {
    object_ownership = "BucketOwnerPreferred"
  }
}
```

## Argument Reference

The following arguments are required:

* `bucket` - (Required) Name of the bucket that you want to associate this access point with.
* `rule` - (Required) Configuration block(s) with Ownership Controls rules. Detailed below.

### rule Configuration Block

The following arguments are required:

* `object_ownership` - (Required) Object ownership. Valid values: `BucketOwnerPreferred`, `ObjectWriter` or `BucketOwnerEnforced`
    * `BucketOwnerPreferred` - Objects uploaded to the bucket change ownership to the bucket owner if the objects are uploaded with the `bucket-owner-full-control` canned ACL.
    * `ObjectWriter` - Uploading account will own the object if the object is uploaded with the `bucket-owner-full-control` canned ACL.
    * `BucketOwnerEnforced` - Bucket owner automatically owns and has full control over every object in the bucket. ACLs no longer affect permissions to data in the S3 bucket.

## Attribute Reference

This resource exports the following attributes in addition to the arguments above:

* `id` - S3 Bucket name.

## Import

Import S3 Bucket Ownership Controls using S3 Bucket name. For example:

```
$ terraform import aws_s3_bucket_ownership_controls.example my-bucket
```
