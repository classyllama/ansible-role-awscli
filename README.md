# Ansible Role: AWS CLI

Installs the AWS CLI tool from downloaded installer on RHEL / CentOS.

Installation instruction reference: https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-linux.html

## Requirements

None.

## Role Variables

See `defaults/main.yml` for details.

## Dependencies

None.

## Example Playbook

    - hosts: all
      roles:
        - { role: classyllama.awscli }

## Notes

  Once installed, some basic commands for testing and using the AWS CLI tool.

    # List the version installed
    aws --version

    # Setup credentials file manually
    aws configure

  Once you have a default credentials file, you can run additional commands to check the resource on AWS

    # List all S3 buckets
    aws s3 ls

  When attempting to list all buckets, if you receive the error:
  
    # An error occurred (RequestTimeTooSkewed) when calling the ListBuckets operation: The difference between the request time and the current time is too large.

  You may need to update the system clock

  If you receive the error:
  
    # An error occurred (AccessDenied) when calling the ListBuckets operation: Access Denied
  
  You should try specifing the bucket name you are trying to list the contents of.
  It's possible you have permission to a specific bucket, but not the ability to list all buckets on the account.

    aws s3 ls s3://<bucket_name>
  
  Using a named profile (using named profiles does not work with rclone)
  
    aws s3 ls s3://<bucket_name> --profile my_profile_name

  List/CopyRemove Examples
  
    aws s3 ls s3://<bucket_name>/
    aws s3 cp <local_file_path> s3://<bucket_name>/
    aws s3 rm s3://<bucket_name>/<object_key_id_file_path>

  List object versions
  
    aws s3api list-object-versions --bucket <bucket_name> --prefix <object_key_id_file_path>
  
  Delete object version (with and without governance bypass)

    aws s3api delete-object --bucket <bucket_name> --key <object_key_id_file_path> --version-id "<version_id>"
    aws s3api delete-object --bucket <bucket_name> --key <object_key_id_file_path> --version-id "<version_id>" --bypass-governance-retention

## License

This work is licensed under the MIT license. See LICENSE file for details.

## Author Information

This role was created in 2020 by [Matt Johnson](https://github.com/mttjohnson/).
