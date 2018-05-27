# CC_Encrypt_AWS

The CC_Encrypt_AWS script is used to convert Rubrik Cloud Clusters on AWS from unencrypted disks to encrypted disks, thus providing encryption at rest for Cloud Cluster in AWS.

## Description

CC_Encrypt_AWS will encrypt both the root disk and the data disks of a freshly launched Rubrik Cloud Cluster. The script can also be used to encrypt a new node that is being added to an existing Cloud Cluster, prior to adding it to the cluster. It is not supported to use this script on a bootstrapped Cloud Cluster at this time. CC_Encrypt_AWS can also be used to re-size the data volume storage of a Cloud Cluster node.

The script will take 40 minutes to an hour to run per node. It can be run in parallel on separate nodes to speed up the process. Each iteration of the script will run on a separate AWS instance. CC_Encrypt_AWS uses this basic methodology to encrypt Cloud Cluster on AWS:

1. Optionally stop the node instance.
2. Create a snapshot of the root volume for the node.
3. Copy the original root volume snapshot to an encrypted snapshot.
4. Create a new volume from the encrypted root volume snapshot copy.
5. Detach the root volume from the node instance.
6. Attach the encrypted root volume to the node instance.
7. Delete the original root volume.
8. Delete the snapshot of the original root volume
9. Delete the snapshot of the encrypted root volume.
10. Detach each empty data volume from the node instance.
11. Create a new encrypted volume of a user specified size.
12. Attach the new encrypted volumes to the node instance using new device names.
13. Optionally start the node instance.

## Prerequisites

- python 3.6.1+ and pip
- boto3
- The node(s) must be deployed but not bootstrapped

## Installation

1. Install [python 3.6.1 and pip](http://docs.python-guide.org/en/latest/starting/installation/) or higher
2. Install [boto3](https://boto3.readthedocs.io/en/latest/guide/quickstart.html)
3. Download and save the [cc_encrypt_aws](https://github.com/rubrik-devops/CC_Encrypt_AWS) script from GitHub to a working directory.

## Usage Instructions

To use the script run `python3 cc_encrypt_aws.py [options]`. Specify the appropriate options for the instance. These include:

```text
usage: cc_encrypt_aws.py [-h] --instanceid IID --disksize DS
                         [--clientmasterkey CMK] [--profile PROFILE]
                         [--stopinstance] [--dryrun]

Encrypt disks for Rubrik Cloud Cluster in AWS

required arguments:

  --instanceid IID, -i IID
                        AWS Instance ID for Rubrik Cloud Cluster node.
  --disksize DS, -d DS  Disk size for disks in nodes. Minimum 512GiB, Maximum
                        2048 GiB. Default is 1024 days.

optional arguments:
  -h, --help            show this help message and exit
  --clientmasterkey CMK, -k CMK
                        Customer Master Key to encrypt volumes. If this is not
                        specified the AWS default key is used.
  --profile PROFILE, -p PROFILE
                        AWS Profile to use. If left blank the default profile
                        will be used.
  --stopinstance, -s    Stop instances if they are running.
  --dryrun, -D          Dry run only. Do not encrypt disks. Default is false.
```

## Future

- Add these functions depending on necessity
  - Include capability to run one script and act on a whole cluster.
  - Add ability to encrypt a running bootstrapped cluster.
  - Support use with CloudFormation
- Clean up the code
  - Break out into multiple files to support CloudFormation integration
  - Standardize methods used in the script

## Contribution

Create a fork of the project into your own repository. Make all your necessary changes and create a pull request with a description on what was added or removed and details explaining the changes in lines of code. If approved, project owners will merge it.

## Licensing

CC_Encrypt_AWS is freely distributed under the [GPLv3 License](https://www.gnu.org/licenses/gpl-3.0.en.html "LICENSE"). See [LICENSE](https://github.com/rubrik-devops/cc_encrypt_aws/blob/master/LICENSE) for details.

## Support

Please file bugs and issues on the Github issues page for this project. This is to help keep track and document everything related to this repo.

### Created and Maintained by the Rubrik Ranger Team

<p align="center">
  <img src="https://user-images.githubusercontent.com/8610203/37415009-6f9cf416-2778-11e8-8b56-052a8e41c3c8.png" alt="Rubrik Ranger Logo"/>
</p>