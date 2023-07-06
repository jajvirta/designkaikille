
## Develop and publish

Install hugo, awscli, aws-vault. Create access keys in AWS Console. Add them
to aws-vault with:

    aws-vault add pdk

Add to ~/.aws/config (where ACCOUNT_ID is the AWS account ID, which you can
find from the AWS Console):

    [profile pdk]
    role_arn=arn:aws:iam::ACCOUNT_ID:role/OrganizationAccountAccessRole
    region=eu-north-1

Verify that it works:

    aws-vault exec pdk -- aws sts get-caller-identity | cat

which should look like:

    {
    "UserId": "...",
    "Account": "...",
    "Arn": "arn:aws:sts::ACCOUNT_ID:assumed-role/OrganizationAccountAccessRole/ID"
    }

Clone the content repository:

    git clone --recurse-submodules --remote-submodules git@github.com:jajvirta/designkaikille.git

Run:

    hugo server --disableFastRender -D

Verify by going to http://localhost:1313

Build and deploy:

    hugo
    aws-vault exec pdk -- hugo deploy

Verify changes by going to https://www.designkaikille.fi/


## Documentation:

Website:
- allow public access to bucket (https://www.reddit.com/r/aws/comments/aay6xt/how_to_serve_up_indexhtml_files_in_subfolders/)
-- eg. point to the website endpoint, not the s3 directly

