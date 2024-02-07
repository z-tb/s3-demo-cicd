# S3 Demo CI/CD

This project demonstrates a simple CI/CD setup using GitHub Actions to automatically upload a JSON file to an S3 bucket. 

## Overview

Whenever a change is pushed to the repository, GitHub Actions triggers a workflow that uploads a JSON file containing contact information to an S3 bucket named in the repository environmnet variable `AWS_S3_TARGET`. The JSON file used in this demo includes ficticious contact details such as FirstName, LastName, City, State, Profession, Phone, and Email. 

Along with the S3 target bucket, the other variables to configure reside under Repository -> `Settings` -> `Environments` -> `probably-the-repo-name`. Of note, the two variables under `Environment secrets` are sensitive and the contents is not available after entereing the value of [Github Secrets](https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions):
  * AWS_ACCESS_KEY_ID
  * AWS_SECRET_ACCESS_KEY

Additionally, the `AWS_REGION` and `AWS_S3_TARGET` are configured as plain-text environment varibles since they are not sensitive.


## Workflow

1. **Trigger**: The workflow is triggered on a push event when changes are made to the `contacts.json` file.

2. **Actions**:
   - **Checkout Repository**: Checks out the repository to the GitHub Actions runner.
   - **Configure AWS Credentials**: Configures AWS credentials using the AWS CLI action.
   - **Upload JSON to S3**: Uses the AWS CLI to copy the `contacts.json` file to the `AWS_S3_TARGET`bucket.

## Usage

1. Ensure you have the required AWS credentials set up with appropriate permissions. These should be long-term credentials issued by campus which are specifically credentialed for ONLY a given IAM role with write access to the specified bucket.
2. Modify the `contacts.json` file to contain the contact information you want to upload.
3. Push the changes to the repository using the `json-upload` branch. This is the branch which implements the S3 upload Action and isolates the Action from other changes, in other branches, of the repository.
4. The GitHub Actions workflow will automatically trigger and upload the `contacts.json` file to the configured S3 bucket.

## License

This project is licensed under the GNU General Public License v3.0 - see the [LICENSE](LICENSE) file for details.
