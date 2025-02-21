---
subcategory: "IAM (Identity & Access Management)"
layout: "aws"
page_title: "AWS: aws_iam_account_password_policy"
description: |-
  Manages Password Policy for the AWS Account.
---

# Resource: aws_iam_account_password_policy

-> **Note:** There is only a single policy allowed per AWS account. An existing policy will be lost when using this resource as an effect of this limitation.

Manages Password Policy for the AWS Account.
See more about [Account Password Policy](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_passwords_account-policy.html)
in the official AWS docs.

## Example Usage

```terraform
resource "aws_iam_account_password_policy" "strict" {
  minimum_password_length        = 8
  require_lowercase_characters   = true
  require_numbers                = true
  require_uppercase_characters   = true
  require_symbols                = true
  allow_users_to_change_password = true
}
```

## Argument Reference

This resource supports the following arguments:

* `allow_users_to_change_password` - (Optional) Whether to allow users to change their own password
* `hard_expiry` - (Optional) Whether users are prevented from setting a new password after their password has expired (i.e., require administrator reset)
* `max_password_age` - (Optional) The number of days that an user password is valid.
* `minimum_password_length` - (Optional) Minimum length to require for user passwords.
* `password_reuse_prevention` - (Optional) The number of previous passwords that users are prevented from reusing.
* `require_lowercase_characters` - (Optional) Whether to require lowercase characters for user passwords.
* `require_numbers` - (Optional) Whether to require numbers for user passwords.
* `require_symbols` - (Optional) Whether to require symbols for user passwords.
* `require_uppercase_characters` - (Optional) Whether to require uppercase characters for user passwords.

## Attribute Reference

This resource exports the following attributes in addition to the arguments above:

* `expire_passwords` - Indicates whether passwords in the account expire. Returns `true` if `max_password_age` contains a value greater than `0`. Returns `false` if it is `0` or _not present_.

## Import

Import IAM Account Password Policy using the word `iam-account-password-policy`. For example:

```
$ terraform import aws_iam_account_password_policy.strict iam-account-password-policy
```
