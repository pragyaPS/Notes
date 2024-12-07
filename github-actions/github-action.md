GitHub Actions is a continuous integration and continuous delivery (CI/CD) platform that allows you to automate your build, test, and deployment pipeline. You can create workflows that build and test every pull request to your repository, or deploy merged pull requests to production.

GitHub Actions goes beyond just DevOps and **lets you run workflows** when other events happen in your repository. For example, you can run a workflow to automatically add the appropriate labels whenever someone creates a new issue in your repository.

GitHub provides Linux, Windows, and macOS virtual machines to run your workflows, or you can host your own self-hosted runners in your own data center or cloud infrastructure.

You can configure a GitHub Actions workflow to be triggered when an event occurs in your repository, such as a pull request being opened or an issue being created. Your workflow contains one or more jobs which can run in sequential order or in parallel. Each job will run inside its own virtual machine runner, or inside a container, and has one or more steps that either run a script that you define or run an action, which is a reusable extension that can simplify your workflow.

A workflow is a configurable automated process that will run one or more jobs. Workflows are defined by a YAML file checked in to your repository and will run when triggered by an event in your repository, or they can be triggered manually, or at a defined schedule.

Workflows are defined in the .github/workflows directory in a repository, and a repository can have multiple workflows, each of which can perform a different set of tasks. For example, you can have one workflow to build and test pull requests, another workflow to deploy your application every time a release is created, and still another workflow that adds a label every time someone opens a new issue.

We can reference a workflow within another workflow. to avoid duplication, easier to maintain and allows to create new workflow more quickly by building on the work of others

About contexts:

Contexts are a way to access information about workflow runs, variables, runner environments, jobs, and steps. Each context is an object that contains properties, which can be strings or other objects

For a workflow to be reusable, the values for on must include workflow_call:

```yaml
on:
  workflow_call:
```

- In the reusable workflow, use the inputs and secrets keywords to define inputs or secrets that will be passed from a caller workflow.

```yml
on:
  workflow_call:
    inputs:
      config-path:
        required: true
        type: string
    secrets:
      envPAT:
        required: true
```

- In the reusable workflow, reference the input or secret that you defined in the on key in the previous step.

> **_NOTE:_**-If the secrets are inherited by using secrets: inherit in the calling workflow, you can reference them even if they are not explicitly defined in the on key. For more

```yml
- uses: ./.github/actions/setup
              with:
                  dh_github_token: ${{ secrets.DH_GITHUB_TOKEN }}
```

> **_NOTE:_** Environment secrets are encrypted strings that are stored in an environment that you've defined for a repository. Environment secrets are only available to workflow jobs that reference the appropriate environment.

- Pass the input or secret from the caller workflow.

To pass named inputs to a called workflow, use the with keyword in a job. Use the secrets keyword to pass named secrets. For inputs, the data type of the input value must match the type specified in the called workflow (either boolean, number, or string).

```yml
jobs:
  call-workflow-passing-data:
    uses: octo-org/example-repo/.github/workflows/reusable-workflow.yml@main
    with:
      config-path: .github/labeler.yml
    secrets:
      envPAT: ${{ secrets.envPAT }}
```
