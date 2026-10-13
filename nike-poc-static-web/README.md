# Nike Product Classification POC — Azure Static Web Apps

A self-contained, browser-only proof of concept for demonstrating Nike product classification. The static site has no build step, backend, API calls, or external runtime assets. Product-image uploads are processed locally in the browser and are not persisted or transmitted.

> **POC notice:** Classification results, confidence scores, and tariff codes are simulated for demonstration purposes only. They are not production trade-compliance determinations.

## Package contents

```text
nike-poc-static-web/
├── index.html  # Application, styles, classification logic, and inline SVG placeholders
└── README.md   # This deployment guide
```

## Validate locally

No dependency installation is required. From this directory:

```bash
python3 -m http.server 8080
```

Open [http://localhost:8080](http://localhost:8080). You can also open `index.html` directly; the POC does not fetch data or assets.

## Deploy to Azure Static Web Apps

### Option A — Azure portal (recommended for a one-time POC deployment)

1. Put this folder in a GitHub repository. Keep `index.html` at the repository root, or in a folder you select as the app location.
2. In the [Azure portal](https://portal.azure.com), select **Create a resource** → **Static Web App**.
3. Choose the Azure subscription, resource group, app name, region, and GitHub deployment source; authorize GitHub when prompted.
4. Select the repository and branch.
5. In **Build Details**, choose a static-site framework such as **Custom**. Set:
   - **App location:** `/` when this package is at the repository root; otherwise the folder that contains `index.html` (for example, `/nike-poc-static-web`)
   - **Api location:** leave blank
   - **Output location:** leave blank
6. Create the resource. Azure adds a GitHub Actions workflow and publishes the site.
7. After the workflow succeeds, open the generated `https://<app-name>.azurestaticapps.net` URL from the Static Web App's **Overview** page. Treat this hostname as the stable internal URL; optionally map an approved internal custom domain in **Custom domains**.

### Option B — Azure CLI

Use the portal/GitHub flow above for repository-connected continuous deployment. If your organization provisions Static Web Apps through the Azure CLI, create the Static Web App resource according to your internal standards, then configure its GitHub Actions workflow with the same build settings: app location at the directory containing `index.html`, and blank API/output locations. This package requires no Node.js build command.

## Post-deployment checks

- Confirm the landing page loads at the `azurestaticapps.net` URL.
- Run each sample scenario (running shoe, training jacket, drawstring backpack) and select **Classify Product**.
- Upload a sample image, verify its preview appears, then clear it.
- Test the **Print / Save as PDF** action in a classification result.
- Verify the browser network panel has no failed asset requests. The site intentionally uses system font fallbacks so it remains usable offline.

## Updating

Edit `index.html`, commit the change, and push to the deployment branch. The Azure-generated GitHub Actions workflow republishes the static site. Do not add credentials, secrets, or production classification data to this POC repository.
