This is a [Next.js](https://nextjs.org) project bootstrapped with [`create-next-app`](https://nextjs.org/docs/app/api-reference/cli/create-next-app).

## Getting Started

#### **Step 1: Prepare Your Next.js Application**

```
npx create-next-app@latest <app-name>
```


#### **Step 2: Integrate GitHub Accounts to the site**

* Go to your Github Account and create a repository. You will provide a GitHub repository URL. 
* Initialize a Git repository in your application folder using the following command:

```
git init
```

* Now, Commit the files using the following command:

```
git add --all
git commit -m "initial commit"
```

* Link your local repository to the remote repository:

```
git remote add origin https://git.abc.com/your-app.git
```

* Finally, Push the code to the remote repository

```
git push origin main
```


#### **Step 3: Create a GitHub Actions Workflow**

* Create a .github/workflows directory in the root of your project if it doesn’t already exist. 

```
mkdir -p .github/workflows
```

* Create a YAML file for your workflow named preview.yml to preview the site.

```
touch .github/workflows/preview.yml
```

* Edit the preview.yml file with the following content:

```
name: Vercel Preview Deployment
env:
  VERCEL_ORG_ID: ${{ secrets.VERCEL_ORG_ID }}
  VERCEL_PROJECT_ID: ${{ secrets.VERCEL_PROJECT_ID }}
on:
  push:
    branches-ignore:
      - main
jobs:
  Deploy-Preview:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Install Vercel CLI
        run: npm install --global vercel@latest
      - name: Pull Vercel Environment Information
        run: vercel pull --yes --environment=preview --token=${{ secrets.VERCEL_TOKEN }}
      - name: Build Project Artifacts
        run: vercel build --token=${{ secrets.VERCEL_TOKEN }}
      - name: Deploy Project Artifacts to Vercel
        run: vercel deploy --prebuilt --token=${{ secrets.VERCEL_TOKEN }}
```

* Now, create another YAML file for your workflow named  production.yml  for production.

```
touch .github/workflows/production.yml
```

* Edit the production.yml file with the following content:

```
name: Vercel Production Deployment
env:
  VERCEL_ORG_ID: ${{ secrets.VERCEL_ORG_ID }}
  VERCEL_PROJECT_ID: ${{ secrets.VERCEL_PROJECT_ID }}
on:
  push:
    branches:
      - main
jobs:
  Deploy-Production:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Install Vercel CLI
        run: npm install --global vercel@latest
      - name: Pull Vercel Environment Information
        run: vercel pull --yes --environment=production --token=${{ secrets.VERCEL_TOKEN }}
      - name: Build Project Artifacts
        run: vercel build --prod --token=${{ secrets.VERCEL_TOKEN }}
      - name: Deploy Project Artifacts to Vercel
        run: vercel deploy --prebuilt --prod --token=${{ secrets.VERCEL_TOKEN }}
```


#### **Step 4: Set Up Vercel Setting in your project**

* At first, Install the Vercel CLI

```
npm i -g vercel
```

* Now run vercel login. This command allows you to log in to your Vercel account through Vercel CLI

```
vercel login
```

* Inside your folder, run vercel link and follow the steps. This will link a local directory to a Vercel Project.

```
vercel link
```

vercel link will generate .vercel folder, save the vercel  projectId and orgId in the project.json

* Finally,  you need to create a vercel deployment token. Go to this [Link](https://vercel.com/account/tokens) to generate a Vercel Token.


#### **Step 5: Set Up Secrets in GitHub**

* Go to your repository on GitHub.
* Click on "Settings".
* Select "Secrets and variables" > "Actions".
* Add the following secrets:

```
VERCEL_TOKEN: Your Vercel deployment token.
VERCEL_ORG_ID: Your Vercel organization ID.
VERCEL_PROJECT_ID: Your Vercel project ID.
```

#### Final Outcome

Once you've set up the workflow file and added your secrets, you can try out the workflow. All you need to do is to create a new pull request to your GitHub repository. GitHub Actions will identify the update and develop your application using the Vercel CLI. T hus it will make a Preview Deployment. A Production build will also be created and deployed once the pull request is merged. A Preview Deployment will be included in every pull request by default. Finally,  Vercel will initiate a fresh Production build to return to the previous git state if the pull request needs to be rolled back. To do this, simply revert and merge the PR.

### Containerization

* Build your container

```
docker build -t nextjs-docker .
```

* Run your container

```
docker run -p 3000:3000 nextjs-docker
```

### Local Development

First, run the development server:

```bash
npm run dev
# or
yarn dev
# or
pnpm dev
# or
bun dev
```

Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.

You can start editing the page by modifying `app/page.tsx`. The page auto-updates as you edit the file.
