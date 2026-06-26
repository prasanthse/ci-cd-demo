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