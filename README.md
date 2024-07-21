# 🐱‍🚀 Astro + Neocities
*This is a Fork of [Deploy-To-Neocities](https://github.com/bcomnes/deploy-to-neocities) adapted for use with Astro and other static site generators.*

## Quickstart

Import the `.github/workflows` folder from this repo into your astro project and configure `neocities.yaml`.
> 💡 *This workflow can be used with any SSG, just note that the build path may be different from Astro's.*

## Usage

```neocities.yaml
name: Deploy to Neocities

concurrency: # prevent concurrent deploys doing strange things
  group: deploy-to-neocities
  cancel-in-progress: true

on:
  push: # will only run on 'git push' to main branch
    branches:
      - main
      # make sure your branch name matches! (ie. main, master, etc)

jobs:
  deploy:
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout the repository code
        uses: actions/checkout@v3
        
      - name: Install Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
        # if the build is throwing node errors, change this to the latest
        # version that is compatible with Astro/your SSG

      - name: Get npm cache directory
        id: npm-cache-dir
        run: echo "dir=$(npm config get cache)" >> $GITHUB_OUTPUT

      - uses: actions/cache@v3
        id: npm-cache
        with:
          path: ${{ steps.npm-cache-dir.outputs.dir }}
          key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
          restore-keys: |
            ${{ runner.os }}-node-
         # caches dependencies and other commonly re-used files
         # so they do not need to be re-installed on each run

      - name: Install dependencies and build site
        run: |
          npm install
          npm run build

      - name: Deploy to Neocities
        uses: bcomnes/deploy-to-neocities@v1
        with:
          api_token: ${{ secrets.NEOCITIES_API_TOKEN }}
          cleanup: true
          dist_dir: dist # if this is within a subfolder, update the filepath to reflect that
```

Generate your Neocities API token at:

```
https://neocities.org/settings/{{your-sitename}}#api_key
```

Then go to your Github Repository Settings and enter your key as a new secret:

```
Settings > Secrets and variables > Actions > New repository secret
```

- Name: NEOCITIES_API_TOKEN
- Secret: `ENTER YOUR API KEY HERE`

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
