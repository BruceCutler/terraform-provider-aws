---
subcategory: "SSM (Systems Manager)"
layout: "aws"
page_title: "AWS: aws_ssm_association"
description: |-
  Associates an SSM Document to an instance or EC2 tag.
---

# Resource: aws_ssm_association

Associates an SSM Document to an instance or EC2 tag.

## Example Usage

### Create an association for a specific instance

```terraform
resource "aws_ssm_association" "example" {
  name = aws_ssm_document.example.name

  targets {
    key    = "InstanceIds"
    values = [aws_instance.example.id]
  }
}
```

### Create an association for all managed instances in an AWS account

To target all managed instances in an AWS account, set the `key` as `"InstanceIds"` with `values` set as `["*"]`. This example also illustrates how to use an Amazon owned SSM document named `AmazonCloudWatch-ManageAgent`.

```terraform
resource "aws_ssm_association" "example" {
  name = "AmazonCloudWatch-ManageAgent"

  targets {
    key    = "InstanceIds"
    values = ["*"]
  }
}
```

### Create an association for a specific tag

This example shows how to target all managed instances that are assigned a tag key of `Environment` and value of `Development`.

```terraform
resource "aws_ssm_association" "example" {
  name = "AmazonCloudWatch-ManageAgent"

  targets {
    key    = "tag:Environment"
    values = ["Development"]
  }
}
```

### Create an association with a specific schedule

This example shows how to schedule an association in various ways.

```terraform
resource "aws_ssm_association" "example" {
  name = aws_ssm_document.example.name

  # Cron expression example
  schedule_expression = "cron(0 2 ? * SUN *)"

  # Single-run example
  # schedule_expression = "at(2020-07-07T15:55:00)"

  # Rate expression example
  # schedule_expression = "rate(7 days)"

  targets {
    key    = "InstanceIds"
    values = [aws_instance.example.id]
  }
}
```

## Argument Reference

This resource supports the following arguments:

* `name` - (Required) The name of the SSM document to apply.
* `apply_only_at_cron_interval` - (Optional) By default, when you create a new or update associations, the system runs it immediately and then according to the schedule you specified. Enable this option if you do not want an association to run immediately after you create or update it. This parameter is not supported for rate expressions. Default: `false`.
* `association_name` - (Optional) The descriptive name for the association.
* `document_version` - (Optional) The document version you want to associate with the target(s). Can be a specific version or the default version.
* `instance_id` - (Optional, **Deprecated**) The instance ID to apply an SSM document to. Use `targets` with key `InstanceIds` for document schema versions 2.0 and above. Use the `targets` attribute instead.
* `output_location` - (Optional) An output location block. Output Location is documented below.
* `parameters` - (Optional) A block of arbitrary string parameters to pass to the SSM document.
* `schedule_expression` - (Optional) A [cron or rate expression](https://docs.aws.amazon.com/systems-manager/latest/userguide/reference-cron-and-rate-expressions.html) that specifies when the association runs.
* `targets` - (Optional) A block containing the targets of the SSM association. Targets are documented below. AWS currently supports a maximum of 5 targets.
* `compliance_severity` - (Optional) The compliance severity for the association. Can be one of the following: `UNSPECIFIED`, `LOW`, `MEDIUM`, `HIGH` or `CRITICAL`
* `max_concurrency` - (Optional) The maximum number of targets allowed to run the association at the same time. You can specify a number, for example 10, or a percentage of the target set, for example 10%.
* `max_errors` - (Optional) The number of errors that are allowed before the system stops sending requests to run the association on additional targets. You can specify a number, for example 10, or a percentage of the target set, for example 10%.
* `automation_target_parameter_name` - (Optional) Specify the target for the association. This target is required for associations that use an `Automation` document and target resources by using rate controls. This should be set to the SSM document `parameter` that will define how your automation will branch out.
* `wait_for_success_timeout_seconds` - (Optional) The number of seconds to wait for the association status to be `Success`. If `Success` status is not reached within the given time, create opration will fail.

Output Location (`output_location`) is an S3 bucket where you want to store the results of this association:

* `s3_bucket_name` - (Required) The S3 bucket name.
* `s3_key_prefix` - (Optional) The S3 bucket prefix. Results stored in the root if not configured.
* `s3_region` - (Optional) The S3 bucket region.

Targets specify what instance IDs or tags to apply the document to and has these keys:

* `key` - (Required) Either `InstanceIds` or `tag:Tag Name` to specify an EC2 tag.
* `values` - (Required) A list of instance IDs or tag values. AWS currently limits this list size to one value.

## Attribute Reference

This resource exports the following attributes in addition to the arguments above:

* `arn` - The ARN of the SSM association
* `association_id` - The ID of the SSM association.
* `instance_id` - The instance id that the SSM document was applied to.
* `name` - The name of the SSM document to apply.
* `parameters` - Additional parameters passed to the SSM document.

## Import

Import SSM associations using the `association_id`. For example:

```
$ terraform import aws_ssm_association.test-association 10abcdef-0abc-1234-5678-90abcdef123456
```
