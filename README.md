These commands and corresponding JSON files are used to import an OVA file from Virtualbox to an AWS instance. Helpful when dev VMs have to be converted to a live instance.

Pre-requisite: Install the AWS CLI to execute the commands on this page. Once installed, make sure it is configured with `aws configure` and follow the prompts. Setup your IAM user ahead of time.

1. Import trust-policy.json to create a VMImport IAM role if not already available.
```
aws iam create-role --role-name vmimport --assume-role-policy-document file://trust-policy.json
```
2. Attach the role-policy.json to that VMimport role
```
aws iam put-role-policy --role-name vmimport --policy-name vmimport --policy-document file://role-policy.json
```
3. Execute the import-image command using the description.json file
```
aws ec2 import-image --description "Ubuntu 16.04 LTS OVA" --disk-containers file://containers.json
```
4. Check on the status of the import using the task-id that is returned by the previous command.
```
aws ec2 describe-import-image-tasks --import-task-ids import-ami-ffx9iabh
```

After it reaches `Progress: 100` you'll get a JSON readout of the details of your new AMI that is ready in your AWS console.
