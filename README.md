s3_puller
=========

This tool pulls content from an AWS S3 bucket. It is intended to help developers of [redhat-cop/agnosticd](https://github.com/redhat-cop/agnosticd) pull down the "output_dir" of deployments.

Requirements
------------
Collections:
- amazon.aws
  
lookup_plugins: 
- [unvault_string.py](https://github.com/redhat-cop/agnosticd/tree/development/ansible/lookup_plugins)

ansible.cfg
-----------
If needed configure ansible.cfg with local parameters for custom lookup plugin locations

```
[defaults]
lookup_plugins = /home/wilson/Projects/s3-puller/lookup_plugins:/opt/ansible/lookup_plugins
vault_password_file = /path/to/vault/key
```

Variables
---------

```yaml
## AWS access vars
s3_puller_access_key: "AWS ACCESS KEY"
s3_puller_secret_key: "AWS SECRET ACCESS KEY"
s3_puller_bucket: "AWS S3 Bucket Name"
s3_puller_region: "AWS Region"

## GUID of deployment, required for quick search of output_dir
guid: "GUID"

## Local destination directory (exists) for output_dir download
s3_puller_local_dir: "../output-dir/"

## Unpack immediately or leave as .tar.gz in {{ s3_puller_local_dir }}
s3_puller_unarchive: True

## If s3_puller_unarchive is true, creates directory and unarchives to location
s3_puller_local_dir_dest: "{{ s3_puller_local_dir}}{{ guid }}"

## Static Vars
## Registered object found based on guid search
_s3_puller_object: "{{ r_s3_objects.s3_keys[0] | default('Key Not Found') }}"

## Destination and name of s3 get
_s3_puller_dest: "{{ s3_puller_local_dir }}{{ r_s3_objects.s3_keys[0] }}"
```
