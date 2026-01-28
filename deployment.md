## Deploying Your Feature Branch to Sandbox
1. Go to your project repository on `GitHub → Settings → Environments → New environment`
   Create an environment named `feature` and click Configure Environment.
2. Under Environment secrets, add the following:
   `DO_DROPLET_IP` (will be provided on request)
   `APP_ENV_FILE` (your app’s full `.env` content)
3. In your project, checkout to a feature branch (preferably feature/deploy).
4. Copy the `infra` folder from any project that already has feature deployment configured.
5. Copy the feature.yaml workflow file from `.github/workflows` in the same project and update the `APP_NAME` variable inside the file to match your app’s name.
6. Commit and push your changes — the pipeline will start automatically.
7. Check the pipeline under Actions in GitHub and fix any errors reported by the workflow.
8. If everything runs successfully, a sandbox feature URL will be generated during the deploy step.
   
## If the sandbox URL loads but your app doesn’t appear
   This means your app hasn’t been exposed through the gateway yet. Do the following:
1. Locate and clone the devops repository.
2. Navigate to `gateway/routes/feature.route.yaml.`
3. Add a service record under services:
   
```bash
    yourappname:
        loadBalancer:
        servers:
            - url: http://yourappname:3000
```

    (You can confirm the correct internal URL from the CD pipeline — check the last 3 runs.)
4. Add a route record under routers:
    
```bash   
    yourappname-feature:
        rule: "HostRegexp(`yourappname-.+.feature.check-dc.com`)"
            service: yourappname
```
5. Commit and push your changes.
6. Restart the gateway pipeline:
   Go to the `devops repo → Actions → Application Gateway → Run workflow`
   Select feature from the dropdown.
