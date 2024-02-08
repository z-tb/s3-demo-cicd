# S3 Demo CI/CD

This project demonstrates a simple CI/CD setup using GitHub Actions to automatically upload a JSON file to an S3 bucket. 

## Overview
The branch `json-upload` is used to isolate a github action to only that branch. The branch is not intended to be merged into `main` as it's only purpose is to run the action to upload `ficticious-contacts.json` to an S3 bucket. Whenever a push is made on that branch, or the file is modified and committed on the `json-upload` branch, the action is triggered.

The action requires the Environment `tb-test`, or another, to be configured with Environment Secrets `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` and Environment Variables `AWS_REGION`. Presumably the AWS account associated with the credentials has appropriately scoped access rules attached to it for only the bucket being uploaded to.

In the workflow file on the `json-upload` branch, there are several variables to configure including the destination S3 bucket. The JSON file used in this demo includes ficticious contact details such as FirstName, LastName, City, State, Profession, Phone, and Email. 

The Environment configuratiojn resides under Repository -> `Settings` -> `Environments` -> `tb-test`. Of note, the two variables under `Environment secrets` are sensitive and the contents not available after entereing the value of [Github Secrets](https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions):
  * AWS_ACCESS_KEY_ID
  * AWS_SECRET_ACCESS_KEY


## Workflow

1. **Trigger**: The workflow is triggered on a push event to the `json-upload` branch, or when changes are made to the `ficticious-contacts.json` file on that branch.

2. **Actions**:
   - **Checkout Repository**: Checks out the repository to the GitHub Actions runner.
   - **Configure AWS Credentials**: Configures AWS credentials using the AWS CLI action.
   - **Upload JSON to S3**: Uses the AWS CLI to copy the `contacts.json` file to the `AWS_S3_TARGET`bucket.

## Usage

1. Ensure you have the required AWS credentials set up with appropriate permissions. These should be long-term credentials issued by campus which are specifically credentialed for ONLY a given IAM role with write access to the specified bucket.
2. Modify the `ficticious-contacts.json` file to contain the contact information you want to upload.
3. Push the changes to the repository using the `json-upload` branch. This is the branch which implements the S3 upload Action and isolates the Action from other changes, in other branches, of the repository.
4. The GitHub Actions workflow will automatically trigger and upload the `ficticious-contacts.json` file to the configured S3 bucket.

## License

This project is licensed under the GNU General Public License v3.0 - see the [LICENSE](LICENSE) file for details.
