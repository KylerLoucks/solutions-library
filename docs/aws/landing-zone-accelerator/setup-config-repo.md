# Setup: Configuration Repository

What you'll do: create and seed the `aws-accelerator-config` repository with sample configuration files and connect it to AWS via CodeStar Connections.

Estimated time: 20–30 minutes

## Prerequisites check

- AWS CLI v2, Git installed
- IAM permissions to create a CodeStar Connection and deploy LZA installer
- Target AWS Region selected (e.g., `us-east-1`)

## 1) Create the config repo and seed sample configs

```bash
# Set your values
export GITHUB_OWNER="your-org-or-username"
export CONFIG_REPO="aws-accelerator-config"
export LZA_VERSION="v1.12.5"

# Create repo locally (or use GitHub UI)
mkdir "$CONFIG_REPO" && cd "$CONFIG_REPO"

# Pull sample configs from LZA repo
curl -L https://github.com/awslabs/landing-zone-accelerator-on-aws/archive/refs/tags/$LZA_VERSION.tar.gz \
  | tar xz
cp -R landing-zone-accelerator-on-aws-$LZA_VERSION/reference/sample-configurations/lza-sample-config/* .

# Initialize repo and push
git init
git add .
git commit -m "seed LZA sample config"
git branch -M main
git remote add origin git@github.com:$GITHUB_OWNER/$CONFIG_REPO.git
git push -u origin main
```

## 2) Create a CodeStar Connection to GitHub

- AWS Console → Developer Tools → Settings → Connections → Create connection
- Choose GitHub → Install a new app when prompted → Finish
- Note the Connection ARN

Reference: [Create a connection](https://docs.aws.amazon.com/dtconsole/latest/userguide/connections-create.html)

## 3) Configure LZA installer parameters

When launching the LZA installer stack, set parameters:

- `UseExistingConfigRepo`: `Yes`
- `ExistingConfigRepositoryName`: `aws-accelerator-config`
- `ExistingConfigRepositoryOwner`: your GitHub org/username
- `ExistingConfigRepositoryBranchName`: `main`
- `ConfigurationRepositoryLocation`: `codeconnection`

Once deployed, the pipeline will consume your config repo to orchestrate landing zone resources.
