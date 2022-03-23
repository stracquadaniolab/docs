# GitHub

The source code of all projects, cookiecutters, and Docker images are stored in
GitHub, under the _stracquadaniolab_ organization. 

## Account setup 

Register a GitHub account at [http://github.com](http://github.com). To be added
to the _stracquadaniolab_ organization, send an email to Giovanni with your
GitHub username. 

## Get a Personal Access Token (PAT)

GitHub requires a Personal Access Token (PAT) to access repositories
programmatically, e.g. when cloning or pulling a repo, or pulling a docker
image. PATs are personal and must not be shared with anyone.

You can obtain a PAT as follows: 

1. Login to GitHub
2. Go to your account `Settings > Developer Settings > Personal access tokens`,
   and click on `Generate a new token`.
3. In the form, put a name, e.g. `Personal PAT`, and select an expiration date,
   e.g. 3 years. Then, select the scopes you want to get access for; it is highly 
   recommended you only tick `repo`, `workflow`, `write:packages`.
4. Click `Generate token` and make sure to save the string, which  you will get
   in the new page and it looks like `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`,
   since it will be shown only once.
