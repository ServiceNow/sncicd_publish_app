# ServiceNow CI/CD GitHub Action for Publish Application

Publishes the specified application and all of its artifacts to the application repository.

# Usage
## Step 1: Prepare values for setting up your variables for Actions
- credentials (username and password for a service account)
- instance URLs for your dev, test, prod, etc. environments
- sys_id or scope for your app
- sys_id for your ATF Test Suite

## Step 2: Configure Secrets in your GitHub repository
On GitHub, go in your repository settings, click on the secret _Secrets_ and create a new secret.

Create secrets called 
- `SNOW_USERNAME`
- `SNOW_PASSWORD`
- `SNOW_SOURCE_INSTANCE` only the **domain** string is required from the instance URL, for example https://**domain**.service-now.com
- `APP_SYS_ID` or `APP_SCOPE`

## Step 3: Example Workflow Template
https://github.com/ServiceNow/sncicd_githubworkflow

## Step 4: Configure the GitHub Action if need to adapt for your needs or workflows
```yaml
- name: Publish Application 
  id: sncicd-publish-app # id of the step
  uses: ServiceNow/sncicd-publish-app@1.0 # like username/repo-name
  with:
      version: "x.x.x"
      versionTemplate: "x.x"
      devNotes: "string"
      versionFormat: exact
  env:
    snowUsername: ${{ secrets.SNOW_USERNAME }}
    snowPassword: ${{ secrets.SNOW_PASSWORD }}
    snowSourceInstance: ${{ secrets.SNOW_SOURCE_INSTANCE }}
    appSysID: ${{ secrets.APP_SYS_ID }}
    appScope: ${{ secrets.APP_SCOPE }}
```
Inputs:
- **version** - Application version to publish. Only for "exact" version format is required
- **devNotes** - Developer notes to store with the application.
- **versionTemplate** - Version template ( like 2.4 ).
- **versionFormat** - exact, detect, autodetect, template
    - **exact** - Use exact version from the input **version** variable
    - **template** - Use x.y.z version where x.y is entered in field below and z is autogenerated
    - **detect** - Try to detect the version from sources. Will fail if no sources found.
    - **autodetect** - Detect the currently installed version and increment it
    
Outputs:
- **rollbackVersion** - The previously installed version.
- **newVersion** - Incremented version. Used to install application step.
    
Environment variable should be set up in the Step 1
- snowUsername - Username to ServiceNow instance
- snowPassword - Password to ServiceNow instance
- snowSourceInstance ServiceNow instance where application is developing
- appSysID - Required if app_scope is not specified. The sys_id of the application
- appScope - Required if app_sys_id is not specified. The scope name of the application, such as x_aah_custom_app

## Tests

Tests should be ran via npm commands:

#### Unit tests
```shell script
npm run test
```   

#### Integration test
```shell script
npm run integration
```   

## Build

```shell script
npm run buid
```

## Formatting and Linting
```shell script
npm run format
npm run lint
```

## Support Model

ServiceNow built this integration with the intent to help customers get started faster in adopting CI/CD APIs for DevOps workflows, but __will not be providing formal support__. This integration is therefore considered "use at your own risk", and will rely on the open-source community to help drive fixes and feature enhancements via Issues. Occasionally, ServiceNow may choose to contribute to the open-source project to help address the highest priority Issues, and will do our best to keep the integrations updated with the latest API changes shipped with family releases. This is a good opportunity for our customers and community developers to step up and help drive iteration and improvement on these open-source integrations for everyone's benefit. 

## Governance Model

Initially, ServiceNow product management and engineering representatives will own governance of these integrations to ensure consistency with roadmap direction. In the longer term, we hope that contributors from customers and our community developers will help to guide prioritization and maintenance of these integrations. At that point, this governance model can be updated to reflect a broader pool of contributors and maintainers. 
