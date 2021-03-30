# Continuous delivery pipelines

This package uses the [AWS Cloud Development Kit (AWS)](https://github.com/awslabs/aws-cdk) to model AWS CodePipeline pipelines and to provision them with AWS CloudFormation.

* pipeline.ts: Builds and publishes the base Docker image for amazon/amazon-ecs-local-container-endpoints.

This creates as CodePipeline pipeline which consists of a souce stage that uses
a GitHub webhook, and build stages that uses AWS CodeBuild to build, publish
and verify Docker images for both amd64 and arm64 architectures to DockerHub.

## GitHub Access Token
The official pipeilne uses a team account (ecs-local-container-endpoints+release@amazon.com).

Create a GitHub [personal access token](https://github.com/settings/tokens) with access to your fork of the repo, including "admin:repo_hook" and "repo" permissions.  Then store the token in Secrets Manager:

```
aws secretsmanager create-secret --region us-west-2 --name EcsDevXGitHubToken --secret-string <my-github-personal-access-token>
```

## Deploy

Any changes to `pipeline.ts` will require a re-compilation and re-deploy.

To deploy this pipeline, install the AWS CDK CLI: `npm i -g aws-cdk`

Install and build everything: `npm install && npm run build`

Then deploy the pipeline stacks:

```
cdk deploy --app 'node pipeline.js'

```

See the pipelines in the CodePipeline console.

**NOTE**: Any changes to `pipeline.ts` will require the stack to be re-build wiht `npm run build` and redeployed with `cdk deploy --app 'node pipeline.js'`
