# Publish-kedro-viz

<picture>
  <source
    srcset="https://raw.githubusercontent.com/kedro-org/kedro/main/.github/demo-light.png"
    media="(prefers-color-scheme: light)"
  />
  <source
    srcset="https://raw.githubusercontent.com/kedro-org/kedro/main/.github/demo-dark.png"
    media="(prefers-color-scheme: dark)"
  />
  <img alt="Shows Kedro logo" src="https://raw.githubusercontent.com/kedro-org/kedro/main/.github/demo-light.png" />
</picture>

<br />
<p align="center">

![Kedro-Viz Pipeline Visualisation](https://raw.githubusercontent.com/kedro-org/kedro-viz/main/.github/img/banner.png)

</p>

<p align="center">
✨ <em> Data Science Pipelines. Beautifully Designed</em> ✨
<br />
Live Demo: <a href="https://demo.kedro.org/" target="_blank">https://demo.kedro.org/</a>
</p>

<br />

## Overview

Publish-kedro-viz is a GitHub action that simplifies the process of deploying Kedro-viz, which is a visual representation of your Kedro project, directly within the Git repository where your Kedro project is stored. By using this action, you can effortlessly showcase your Kedro-viz on GitHub Pages.

This action helps in the automation of a deployment strategy mentioned in [platform agnostic sharing with Kedro-Viz](https://docs.kedro.org/projects/kedro-viz/en/v8.0.1/platform_agnostic_sharing_with_kedro_viz.html#static-website-hosting-platforms-such-as-github-pages)

## Prerequisites

- **GitHub Repository:** A GitHub repository with your Kedro project.
- **GitHub Pages Setup:** Configure your repository for [GitHub Pages](https://docs.github.com/en/pages/quickstart).
- **Kedro-project dependencies:** Install all the Kedro-project dependencies before using this action in your workflow.
- **Python-version:** You need to have an environment with `python>=3.9` in your workflow.

> [!NOTE]
> While configuring your repository for GitHub Pages, you have two publishing source options. You can either choose a branch or a custom GitHub Actions workflow (recommended). 
> If you choose a branch, the build artifacts will be uploaded to the `publish_branch` you pass as an input to the action, which defaults to `gh-pages`. 
> If you choose a custom GitHub Actions workflow, you need to mention that in the input `publishing_source` to the action (this is made as default from v2). In this case, no branch will be created and the artifacts are deployed at run-time. 

> [!TIP]
> Please find more information on configuring a publishing source for github pages site in the [official docs](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site).


## Usage

1. With `kedro-org/publish-kedro-viz@v1`:

  ```yaml

  - uses: kedro-org/publish-kedro-viz@v1
    with:
      # The GitHub token to authenticate deployment. This is autogenerated by the action.
      # Default: ${{ github.token }}
      github_token: ''

      # The Kedro-project path to build the Kedro-Viz artifacts.
      # Default: '.'
      project_path: ''

      # The flag to include hooks while creating your Kedro-project build artifacts.
      # Default: false
      include_hooks: ''
      
      # Your consent to participate in Kedro-Telemetry.
      # Default: true
      telemetry_consent: ''

      # The publishing source for GitHub pages. This can be either 
      # 'branch' or 'workflow' based on your GitHub Pages configuration
      # Default: 'branch'
      publishing_source: ''

      # The GitHub pages publish branch to upload the artifacts 
      # if your publishing_source is a branch
      # Default: 'gh-pages'
      publish_branch: ''

      # The commit message for the deployment, if your publishing_source is a branch.
      # Defaults to your original commit message.
      # Default: ${{ github.event.head_commit.message }}
      commit_message: ''

      # An option to publish branch with only the latest commit
      # if your publishing_source is a branch.
      # Default: true
      force_orphan: ''

      # The git config user.name or the owner of the commit.
      # if your publishing_source is a branch.
      # Default: 'github-actions[bot]'
      user_name: ''

      # The git config user.email or the email of the commit owner.
      # if your publishing_source is a branch.
      # Default: 'github-actions[bot]@users.noreply.github.com'
      user_email: ''

  ```

> [!Important]
> From `publish-kedro-viz@v2`, we recommend you to choose a custom GitHub Actions workflow as a publishing source for GitHub Pages. No build artifacts will be pushed to a branch starting from `publish-kedro-viz@v2`.

2. With `kedro-org/publish-kedro-viz@v2`:

  ```yaml

  - uses: kedro-org/publish-kedro-viz@v2
    with:
      # The Kedro-project path to build the Kedro-Viz artifacts.
      # Default: '.'
      project_path: ''

      # The flag to include hooks while creating your Kedro-project build artifacts.
      # Default: false
      include_hooks: ''
      
      # Your consent to participate in Kedro-Telemetry.
      # Default: true
      telemetry_consent: ''
    
  ```

## Configure the action

1. Adding the GitHub Action to your workflow:

   1. Create a workflow file in your repository: `.github/workflows/publish-kedro-viz.yml`
   2. Add the following code to the workflow file:

    ```yaml
        # An example workflow configuration

        # The name of your workflow
        name: Publish and share Kedro Viz 
        
        permissions:
            
            # The contents write permission is required to use the action 
            # if your GitHub publishing source is a branch
            contents: write 
            
            # The pages and id-token write permissions are required to use 
            # the action if your GitHub publishing source is a custom 
            # GitHub Actions workflow
            pages: write 
            id-token: write
        
        on: 
            # This can be configured based on your requirements 
            # (i.e., the workflow trigger condition)
            pull_request:
            push:
                branches:
                    - main
            workflow_dispatch:

        # We mentioned the minimal jobs for the workflow
        jobs: 
            deploy:
                # The action is currently tested on ubuntu-latest (Recommended)
                runs-on: ubuntu-latest 
                steps:
                    - name: Fetch the repository
                      uses: actions/checkout@v4
                    - name: Set up Python
                      uses: actions/setup-python@v5
                      with:
                        # Requires python >= 3.9
                        python-version: 3.11 
                      # This installs the Kedro-project dependencies
                    - name: Install Project Dependencies
                      run: |
                        python -m pip install --upgrade pip
                        # This is not required if your Kedro Project 
                        # is at the root of your GitHub Repository
                        cd demo-project 
                        pip install -r requirements.txt
                      # Using the action
                    - name: Deploy Kedro-Viz to GH Pages 
                      uses: kedro-org/publish-kedro-viz@v1
                      with:
                        # This is not required if your Kedro Project 
                        # is at the root of your GitHub Repository
                        project_path: 'demo-project'     
    ```

## Test the action

After you've completed the configuration, trigger the workflow as per the workflow trigger condition.

- Once triggered, the GitHub workflow "Publish and share Kedro Viz" will automatically start and can be found in the Actions tab with your commit message.
- If your GitHub Pages publishing source is a custom GitHub Actions workflow (recommended), then the artifacts of your Kedro-Viz static site will be uploaded and deployed during the workflow run-time.
- If your GitHub Pages publishing source is a branch, then the artifacts of your Kedro-Viz static site will be uploaded to the publish-branch input specified in the action upon successful completion of the workflow. Please note that starting v2, we will not upload artifacts to a branch. You can still use publishing source as a branch for GitHub Pages (not recommended) and make sure you follow the security considerations mentioned [here](https://github.com/actions/deploy-pages?tab=readme-ov-file#security-considerations).
- You can access the static site at `http://<username>.github.io/<repo-name>`, if your site's visibility is public. For more information on changing the visibility, you can follow the [official docs](https://docs.github.com/en/enterprise-cloud@latest/pages/getting-started-with-github-pages/changing-the-visibility-of-your-github-pages-site)

## Credits

The list of third party actions used in this project, with due credits to their authors and license terms. More details can be found inside the folder of each action.

### Deploy to GitHub Pages when publishing source is a branch

We use the GitHub action [peaceiris/actions-gh-pages](https://github.com/peaceiris/actions-gh-pages) in v1, to deploy the static site to a publish branch which is released under MIT license.

### Deploy to GitHub Pages when publishing source is a custom GitHub Action Workflow

We use the GitHub actions [actions/upload-pages-artifact](https://github.com/actions/upload-pages-artifact) and [actions/deploy-pages](https://github.com/actions/deploy-pages) which are released under MIT license.

## License

publish-kedro-viz is licensed under the [Apache 2.0](https://github.com/kedro-org/publish-kedro-viz/blob/main/LICENSE.md) License.