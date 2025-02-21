---
subcategory: "CloudFront"
layout: "aws"
page_title: "AWS: aws_cloudfront_public_key"
description: |-
  Provides a CloudFront Public Key which you add to CloudFront to use with features like field-level encryption.
---

# Resource: aws_cloudfront_public_key

## Example Usage

The following example below creates a CloudFront public key.

```terraform
resource "aws_cloudfront_public_key" "example" {
  comment     = "test public key"
  encoded_key = file("public_key.pem")
  name        = "test_key"
}
```

## Argument Reference

This resource supports the following arguments:

* `comment` - (Optional) An optional comment about the public key.
* `encoded_key` - (Required) The encoded public key that you want to add to CloudFront to use with features like field-level encryption.
* `name` - (Optional) The name for the public key. By default generated by Terraform.
* `name_prefix` - (Optional) The name for the public key. Conflicts with `name`.

**NOTE:** When setting `encoded_key` value, there needs a newline at the end of string. Otherwise, multiple runs of terraform will want to recreate the `aws_cloudfront_public_key` resource.

## Attribute Reference

This resource exports the following attributes in addition to the arguments above:

* `caller_reference` - Internal value used by CloudFront to allow future updates to the public key configuration.
* `etag` - The current version of the public key. For example: `E2QWRUHAPOMQZL`.
* `id` - The identifier for the public key. For example: `K3D5EWEUDCCXON`.

## Import

Import CloudFront Public Key using the `id`. For example:

```
$ terraform import aws_cloudfront_public_key.example K3D5EWEUDCCXON
```
