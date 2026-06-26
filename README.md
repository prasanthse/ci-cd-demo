# CI/CD PIPELINE SETUP INSTRUCTIONS

Follow these steps to configure or update the CI/CD pipeline for this project:
1. **Configure Triggers**: Update the ```on``` section in ```.github/workflows/deploy.yml``` to specify the branches that should trigger the pipeline ```(e.g., main, development)```.

2. **Specify Node.js Version**: Ensure the ```node-version``` in the ```Setup Node.js``` step matches the version required by your project.

3. **Define Environment Metadata**: In the ```Configure Build Environment``` step, define how the pipeline should handle versioning and environment tagging for your specific deployment targets.

4. **Inject Environment Variables**: In the ```Build Application``` step, map all necessary environment variables to the build process.
  - ***Note***: Use GitHub Repository Secrets to handle sensitive data
  - To ensure your pipeline functions correctly and securely, it is essential that you add your environment variables to the repository settings rather than the code itself.

## How to Create Environments in GitHub

If you have multiple environments ```(e.g., Staging and Production)```, skip this section

1. Go to your Repository ```Settings```.

2. On the left sidebar, click ```Environments```.

3. Click New environment. Create your environments. ```(e.g., staging and production)```.

## How to Configure Repository Secrets

If you have a single environment:
1. **Navigate to Settings**: Go to your GitHub repository and click on the ```Settings tab```.

2. **Access Secrets**: In the left sidebar, click on ```Secrets and variables -> Actions```.

3. **Create New Secret**: Click the ```New repository secret button```.

4. **Add Your Keys**: For every entry in your ```.env``` files ```(e.g., VITE_SOME_API_KEY)```, create a corresponding secret:
  - **Name**: Copy the exact variable name ```(e.g., VITE_SOME_API_KEY)```.
  - **Secret**: Paste the actual value from your local .env file.
  - Click Add secret.

If you have multiple environments ```(e.g., Staging and Production)```:
1. Once the environments are created, click on each one ```(e.g., click staging)```:

2. Scroll down to ```Environment secrets```.

3. Click Add secret.

4. Add your env variables with the specific environment value here.

5. Repeat for the other environments, adding the same variable name but with the environment-specific value.

## How to Configure Repository Variables

If you have a single environment:
1. Go to ```Settings > Secrets``` and ```variables > Actions```.

2. Click the ```Variables tab```.

3. Click ```New repository variable```.

4. Enter the Name and Value for your configuration setting.

5. Click Add variable.

If you have multiple environments ```(e.g., Staging and Production)```:
1. Go to ```Settings > Environments``` and click on your environment ```(e.g., staging)```.

2. Scroll down to Environment variables.

3. Click Add variable.

4. Enter the Name ```(e.g., VITE_ENVIRONMENT_NAME)``` and the Value ```(e.g., Staging)```.

5. Repeat for the other environments, adding the same variable name but with the environment-specific value.

## Enable GitHub Pages

1. Go to your repository on GitHub

2. Click on the ```Settings tab```.

3. On the left sidebar, scroll down to the "Code and automation" section and click ```Pages```.

4. Under Build and deployment > Source, change the dropdown from ```"Deploy from a branch"``` to ```"GitHub Actions"```.

5. Once you select "GitHub Actions", GitHub will automatically configure the necessary permissions to allow your deploy.yml to push artifacts to the Pages service.

  - ***Note***: Vite projects default to looking for files at the root (/). However, GitHub Pages often hosts your site at https://yourusername.github.io/your-repo-name/. Because your site is in a sub-folder (/your-repo-name/), your browser looks for https://yourusername.github.io/index.html instead of the correct path. The Fix: Open your vite.config.js and add the base property.

## What to Do If You Don't Want Deployment

If you are using this pipeline template for a backend project (like Node.js) or just want to run the automated build/validation steps without any hosting integration:

- Keep the Script As Is: You can leave the pipeline exactly as it is without breaking anything. When you run the workflow manually, simply leave the ```Select Deployment Target dropdown``` set to ```none```. The pipeline will safely compile and test your code but will completely skip the deploy job.

- (Optional) Completely Remove the Deployment Job: If your project will never need GitHub Pages or Firebase, open ```.github/workflows/deploy.yml```, scroll down to the ```deploy: block``` at the bottom, and delete that entire section. Your pipeline will then function purely as a CI validator.

## Deploy New Version

### Deploy to GitHub Pages (With Staging & Production URLs)

1. Open your vite.config.js file and update it to conditionally similar to this demo project's config

2. Push the change to your repository: Go to your GitHub repository page.

3. Click the ```Actions tab```.

4. On the left-hand sidebar, click on your pipeline name.

5. You will see a banner that says: ```"This workflow has a workflow_dispatch trigger."```

6. Click the ```Run workflow button``` (on the right side).

7. A dropdown menu will appear where you can select patch, minor, or major.

8. Choose your ```target branch```, and change the ```Select Deployment Target``` dropdown to ```github```.

7. Click the ```green Run workflow button```.

  - ***Note***: GitHub's UI is strict: The .yml file containing the workflow_dispatch code must exist on your default branch (usually main) before the button will show up. Even if you want to run the workflow on other branches, GitHub won't display the button anywhere until it sees that workflow_dispatch trigger merged into main. The Fix: Push or merge your updated .yml file into your main branch. Once it is there, the button will appear, and you can use it to target any branch you want.

### Deploy to Firebase (Multiple Targets)

1. Configure your project with firebase

2. Generate the ```FIREBASE_SERVICE_ACCOUNT``` Token
  - Open the ```Google Cloud Console``` or the ```Firebase Console -> Project Settings -> Service Accounts```.

  - Click ```Manage service account permissions```.

  - Locate or create a service account with the Firebase Hosting Admin role.

  - Click on the ```service account```, go to the ```Keys tab```, click ```Add Key -> Create new key, and select JSON```.

  - A .json file containing your private token will download to your computer.

3. Push the change to your repository: Go to your GitHub repository page.

4. Click the ```Actions tab```.

5. On the left-hand sidebar, click on your pipeline name.

6. You will see a banner that says: ```"This workflow has a workflow_dispatch trigger."```

7. Click the ```Run workflow button``` (on the right side).

8. A dropdown menu will appear where you can select patch, minor, or major.

9. Choose your ```target branch```, and change the ```Select Deployment Target``` dropdown to ```firebase```.

10. Click the ```green Run workflow button```.

> ⚠️ **CRITICAL ARCHITECTURE NOTE: MULTI-ENVIRONMENT HOSTING**
> 
> **GitHub Pages** natively supports only **one active deployment per repository**. 
> * If you select the `github` target while on the `development` branch, it will overwrite your live production site.
> * If you select the `github` target while on the `main` branch, it will overwrite your staging site.
>
> **The Recommendation:**
> If your project requires true, simultaneous multi-environment deployments (where both Staging and Production URLs must be online at the same exact time), **do not use GitHub Pages for both targets**. 
> 
> Instead, we highly recommend integrating specialized modern hosting platforms that natively support parallel environment tracking, such as:
> * **Firebase Hosting** (Excellent for Google Ecosystem integrations and custom multi-site targets)
> * **Vercel** (The industry standard for frontend frameworks like Next.js/Vite with instant preview branches)
> * **Netlify** (Incredibly developer-friendly with robust forms, serverless functions, and production splits)
> * **AWS Amplify / Cloudflare Pages** (Best for enterprise-level scaling, edge routing, and deeper cloud architectures)
> 
> **How to mix them:** Use **GitHub Pages** exclusively for your `main` branch (Production) and hook your `development` branch (Staging) up to one of the providers listed above via your dropdown options!