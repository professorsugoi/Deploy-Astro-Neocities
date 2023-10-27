# 🐱‍🚀 Astro + Neocities
*This is a Fork of [Deploy-To-Neocities](https://github.com/bcomnes/deploy-to-neocities) adapted for use with Astro and other static site generators.*

## Quickstart

Import the `.github/workflows` folder from this repo into your astro project and rename `neocities.yaml.test` to `neocities.yaml`.
> 💡 *This workflow can be used with any SSG, just note that the build path may be different from Astro's.*

## Usage

```neocities.yaml
name: Deploy to Neocities

# only run on changes to master
on:
  push:
    branches:
      - master # change to your branch name if different. (ie. main)

concurrency: # prevent concurrent deploys doing strange things
  group: deploy-to-neocities
  cancel-in-progress: true

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout the repository code
        uses: actions/checkout@v3
      - name: Install Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18' # if the build is throwing node errors, change this to the latest version that is compatible with Astro/your SSG

      - name: Install dependencies and build site
        run: |
          npm install
          npm run build

      # When the dist folder is ready, deploy it to neocities
      - name: Deploy to Neocities
        uses: bcomnes/deploy-to-neocities@v1
        with:
          api_token: ${{ secrets.NEOCITIES_API_TOKEN }}
          cleanup: true
          dist_dir: dist

      #- name: Push changes to GitHub
      #  run: |
      #    git config user.email "YOUR_EMAIL_HERE"
      #    git config user.name "YOUR_USERNAME_HERE"
      #    git add .
      #    git commit --allow-empty -m "actions: deploy + commit"
      #    git push

      ## uncomment this block if you want to push commits to neocities *and* your github repo
      ## see the README "optional" step for more information
```

Generate your Neocities API token at:

```
https://neocities.org/settings/{{your-sitename}}#api_key
```

Then go to your Github Repository Settings and enter your key as a new secret:

```
Settings > Secrets and variables > Actions > New repository secret
```

- Name: NEOCITIES_AP_TOKEN
- Secret: `ENTER YOUR API KEY HERE`


## Optional

Enable permissions for Github Actions to write to your repo _in addition_ to deploying to Neocities.

```
Settings > Actions > General
```

- Scroll down to the `Workflow permissions` and tick the box `Read and Write Permissions`.


## Inputs

`api_token`: (REQUIRED): The API token for your Neocities website to deploy to.

`dist_dir`: The directory deployed to Neocities, and Astro's build directory. Do not change this unless you also change Astro's build path.  
Default: `dist`

`cleanup`: If true, `deploy-to-neocities` will delete orphan files on Neocities not found in `dist` or `public`.  
Default: `false`

`protected_files`: (OPTIONAL) Used to mark files as protected. Protected files are never cleaned up. **Test this option out with cleanup set to false before relying on it.** Protected files can still be updated.

## Resources

- [Host your own site on Neocities!](https://neocities.org/)
- [Deploy-To-Neocities Docs](https://github.com/bcomnes/deploy-to-neocities)
- [Astro Docs](https://docs.astro.build) / [Discord](https://astro.build/chat)

## ( `ε´ ) Note!

I made this repo to learn more about Astro and Github Actions, and also for my own convenience.  
I'm still a novice at coding, so I'll try to make this better if I can. Outside contributions are also welcome!

If you find this useful, please go star the original [Deploy-To-Neocities](https://github.com/bcomnes/deploy-to-neocities) repo ⭐~
