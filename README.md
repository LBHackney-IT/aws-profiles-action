# aws-profiles-action

Creates an AWS Profile with the specified role arn and profile name, if no profile name is given the default profile is used.

# Usage
## Inputs
```YAML
  aws_region: 
    description: Region to assume role with
    required: false
    default: eu-west-2
  aws_access_key: 
    description: AWS access key
    required: true
  aws_secret_key:
    description: AWS secret key
    required: true
  aws_profile_name:
    description: Profiles for each role to be created
    required: false
    default: default
  role_arn:
    description: role arn to assume
    required: true
  role_session_duration:
    description: "Duration of time in seconds before the token expires. Defaults to 1 Hour"
    default: 3600
    required: false
```
## Example Workflow
```YAML
jobs:
  DeployDev:
    name: Deploy to Dev 
    if: startsWith(github.ref_name, 'feature/')
    runs-on: ubuntu-latest
    steps:
      - name: checkout
        uses: actions/checkout@v3
      - name: assume roles
        uses: LBHackney-IT/aws-profiles-action@main
        with:
          aws_access_key: ${{ secrets.AWS_ACCESS_KEY }}
          aws_secret_key: ${{ secrets.AWS_SECRET_KEY }}
          role_arn: arn:aws:iam::${{ secrets.ACCOUNT_ID }}:role/ROLE
          aws_profile_name: test_account_role
          aws_region: eu-west-2
      - name: deploy 
        uses: LBHackney-IT/terraform-action@main
        with: 
          github_token: ${{secrets.GITHUB_TOKEN}}
          backend_config: 'backend/config.dev.tfbackend'
          vars_file: 'tfvars/dev.tfvars'
```
