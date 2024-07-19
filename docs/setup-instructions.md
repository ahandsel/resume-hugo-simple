# Setup Instructions


## Table of Content <!-- omit in toc -->

* [Create a .env](#create-a-env)
* [Create a PDF.co Account](#create-a-pdfco-account)
  * [Create a Netlify Account](#create-a-netlify-account)
  * [Configure the Repository](#configure-the-repository)
  * [Configure Your Resume](#configure-your-resume)
* [GitHub Actions](#github-actions)
* [Credits](#credits)

---


### Fork the GitHub Repository

Fork the [ahandsel/resume-hugo-simple](https://github.com/ahandsel/resume-hugo-simple) repository.


## Create a .env

Use the [.env.example](../.env.example) file to create a `.env` file in the root of your repository. This file will hold your sensitive information, such as API keys and personal access tokens.

You will need to add the secret variables to your GitHub repository settings later on.


## Create a PDF.co Account

Sign up for an account on [PDF.co](https://pdf.co/) and head to the [Dashboard](https://app.pdf.co/).  
The initial free credits should be enough for this project but you can always buy more if you need them.

On the dashboard, select "View Your API key" and save it in the .env file in the root of your repository.

The generation process itself is done using a simple API call to pdf.co. The code for this resides in the `get_pdf.py` file:


### Create a Netlify Account

Sign up for an account at [Netlify](https://netlify.com/). Netlify is a hosting service for static sites and offers a solid free tier.

When creating an account, you will be asked to link your GitHub account. This is not necessary for this project.

Instead, select the manual deployment option. This will create a new site for you.  
Click on "Deploy manually" in the "Add new site" dropdown. Upload an empty `index.html` file to just get started.

In the config section for your new site, click on "Site settings" and copy your "Site ID".  
Save this ID in the .env file in the root of your repository.


Next, head over to your [user's settings](https://app.netlify.com/user/applications) and create a personal access token.  
Save this token in the .env file in the root of your repository.


### Configure the Repository

Next, head over to the Repository settings page.


Create a new repository secret called `NETLIFY_PERSONAL_ACCESS_TOKEN` and put your Netlify personal access token in here.

Next, create another repository secret called `PDFCO_KEY` and paste your PDF.co API key.

In the "Variables" tab, create two repository variables:

`NETLIFY_SITE_ID` with the site ID from your Netlify settings, and `RESUME_URL` with the URL of your resume. It's okay to use Netlify's URL (something like `majestic-chaja-1234.netlify.app`) here for the moment.

In your GitHub's repository settings, head over to "Actions" > "General" and make sure that "Read and write permissions" are set in the "Workflow Permissions" section.


### Configure Your Resume

Next, check out your forked repository and open it with your favourite IDE. The only file you need to edit is `confg.toml` which includes all the sections of your resume.

Once you commit and push your changes, the magic will start: GitHub Actions will wind a machine up to build your resume using hugo, upload it to netlify, have pdf.co create the PDF versions, then upload the PDFs to Netlify as well, and finally commit the final PDFs to your repos. Let's see how that works in detail.


## GitHub Actions

GitHub Actions are a free GitHub feature that automates tasks around your code.

We will use it to generate a static website using HUGO, create PDF versions of the resume, and upload everything to Netlify.

Again, we see the usage of repository secrets and variables inside the Python script. We can access particular values with `os.environ.get("`. This way, we do not share sensitive information, such as API keys in our code.

The last steps of our job upload the fresh PDF files to Netlify, and pushes the changes to our repo.


## Credits

The Resume itself is based on the phantastic [DevCard](https://themes.3rdwavemedia.com/bootstrap-templates/resume/devcard-bootstrap-5-vcard-portfolio-template-for-software-developers/) theme by Xiaoying Riley.

The instructions are based on the article [Build An Online Resume with an Auto-Updating PDF Version Using GitHub Actions](https://bas.codes/posts/github-actions-resume) by Bas Peters.
