# Deployment Pusher

## Description

The **Deployment Pusher** GitHub Action updates a deployment file in another repository using GitHub App authentication. This action is ideal for workflows that require updating deployment configurations in a separate repository, ensuring that your continuous deployment process is streamlined and secure.

## Inputs

- `repo-url`: The GitHub repository URL to update. (**Required**)
- `environment`: The deployment environment (e.g., staging or production). (**Required**)
- `namespace`: The Kubernetes namespace for the app. (**Required**)
- `app-name`: The name of the application. (**Required**)
- `new-image-name`: The name of the new image. This is the name we will insert in the deployment file. (**Required**)
- `github-app-id`: The GitHub App ID. (**Required**)
- `github-app-private-key`: The GitHub App private key. Ensure to handle with care and use secrets. (**Required**)
- `installation-id`: The Installation ID for the GitHub App on the target repository. (**Required**)
- `deployment-file`: The path to the deployment file to update in the GitOps repo. If this is not specified, then we will calculate the path. (**Optional**)

## Usage

To use this action, add it to your workflow `.yml` file with the necessary inputs. Below is an example of how to integrate the **Deployment Pusher** action into your GitHub Actions workflow:

```yaml
jobs:
  update-deployment:
    runs-on: ubuntu-latest
    steps:
      - name: Push Deployment Update
        uses: likvido/action-deployment-pusher@v2.1
        with:
          repo-url: 'https://github.com/target-repository-owner/target-repository'
          environment: 'production'
          namespace: 'my-namespace'
          app-name: 'example-app'
          new-image-name: 'my-image-name'
          github-app-id: ${{ secrets.GITHUB_APP_ID }}
          github-app-private-key: ${{ secrets.GITHUB_APP_PRIVATE_KEY }}
          installation-id: ${{ secrets.INSTALLATION_ID }}
          deployment-file: 'path/to/deployment.yml'
```

## Important Notes

- **Security**: Store sensitive information like `github-app-private-key` and other secrets securely in GitHub Secrets.
- **Permissions**: Make sure your GitHub App has the necessary permissions to perform operations on the target repository.
- **GitHub App Authentication**: This action uses a GitHub App for authentication to ensure secure and scoped access to the target repository.

For more detailed information about creating and managing GitHub Apps, please refer to the [GitHub Developer documentation](https://developer.github.com/apps/).

# Releasing new version

Either create the new release + new version tag directly in the Github UI, or create it like this:

1. Commit and push your changes
2. Create a new tag: `git tag -a -m "Description of this release" <version>`
3. Push the tag: `git push --follow-tags`

## Example

```
git tag -a -m "First version" v1
git push --follow-tags
```