# Building and Deploying a build of the Web Application

## Set up your local deployment repository

1. Ensure you have followed the [[main build instructions]] to set up the initial web application build artifacts and directories.
1. Navigate to `C:\src\azure`
1. Clone the Azure App Service repo in to the BrightmetricsWeb folder
   `git clone https://brightmetrics-staging.scm.azurewebsites.net:443/brightmetrics.git BrightmetricsWeb`
1. Change directory in to `C:\src\azure\BrightmetricsWeb`
1. Add the additional deployment repositories to the remotes list
    - **TESTING:** `git remote add testing https://brightmetrics-testing.scm.azurewebsites.net/brightmetrics-testing.git`
    - **BETA:** `git remote add beta https://brightmetrics-beta.scm.azurewebsites.net:443/brightmetrics-beta.git`
    - **HARBOR:** `git remote add harbor https://harbor-analytics-staging.scm.azurewebsites.net:443/harbor-analytics.git`
1. Add a branch for beta and testing
    - `git fetch beta`, then `git branch beta beta/master`
    - `git fetch testing` then `git branch testing testing/master`

## Deploy the Web Application

1. Navigate to `C:\src\azure\BrightmetricsWeb\`
1. Change to the branch for the deployment you will be pushing
   - e.g. `git checkout beta`
   - If there are conflicting unstaged changes or extra files, discard them with `git reset --hard` and `git clean -fd` as needed.
   - If you think someone else may have pushed to this deployment target since you last did, you can do, e.g., `git fetch beta` and `git reset --hard beta/master` now.  Otherwise, you can come back to this step if you get push conflicts at the end.
1. Build the Web Application using the [[main build instructions]], adding "release" as an argument to the build.cmd script (`.\build.cmd release`)
1. Back in `C:\src\azure\BrightmetricsWeb`, run `git status` to make sure all the changed/added files are expected.  Run `git diff` or `gitk` if needed for verification.
1. Run the script to prepare for deployment
    - `..\..\<projects repo>\Config\updateWebConfig.ps1` (if using command shell then `powershell ..\..\<projects repo>\Config\updateWebConfig.ps1` instead). Note: this cmdlet assumes a directory `c:\src\azure\bugsnag` exists, as well as a file within it named `build.json`. If you don't have that/those, just initialize them to satisfy the command.
1. Add added/changed files to the current commit
    - `git add -A`
1. Commit the build using the last commit hash from the branch you built the web application from
    - `git commit -m "Release build <version> from <commit-hash>"`
    - The commit hash can be gathered from running `git log` in the base of the web application repository
    - The version is determined based on the current release cycle.  For example, if this is the first deployment of 2020.12 it would be 2020.12.0.  If it is the next deployment of 2020.12 for hotfixes, it would be 2020.12.1.  On beta and testing the deployment will typically be pre-release and the version can be skipped.
    - For production deployments, the corresponding commit from which the deployment was built should be tagged as, e.g., `git tag webapp-2020.12.0` and pushed to Github `git push origin webapp-2020.12.0`.
1. Push to the remote that you are deploying to
    - **TESTING:** `git push testing testing:master`
    - **BETA:** `git push beta beta:master`
    - **PRODUCTION:** `git push`
    - **HARBOR:** `git push harbor`

### If pushing a build to **Production**

1. Log into staging.brightmetrics.com to review it works and to warm up the instance
1. Go to portal.azure.com > app services > all subscriptions > brightmetrics
1. Swap the Staging and Production instances

### If pushing a build to **Harbor**

Same as above, except the harbor-analytics app service, validated via the URL https://harbor-analytics-staging.azurewebsites.net/

A new build is not needed for Harbor, typically the deployment to Harbor will be done after the first hotfix release for the main deployment has been released for a few days, so it's just a matter of pushing to the `harbor` remote, validating, and swapping live.

