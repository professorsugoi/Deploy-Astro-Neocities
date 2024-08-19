# 🐱‍🚀 Astro + Neocities
*This is a Fork of [Deploy-To-Neocities](https://github.com/bcomnes/deploy-to-neocities) adapted for use with Astro and other static site generators.*

## Quickstart

Import the `.github/workflows` folder from this repo into your astro project and configure `neocities.yaml`.
> 💡 *This workflow can be used with any SSG, just note that the build path may be different from Astro's.*

## Usage

```neocities.yaml
name: Deploy to Neocities

env:
  FORCE_COLOR: 1
  NODE_VERSION: lts/*

concurrency:
  group: deploy-to-neocities
  cancel-in-progress: true

on:
  push:
    branches:
      - main
      # make sure your branch name matches! (ie. main, master, etc)

jobs:
  deploy:
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout the repository code
        uses: actions/checkout@v4
        
      - name: Install Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
        # this will use the latest LTS version of Node.js
        # if you need a specific version, replace ${{ env.NODE_VERSION }} with that version number
        
      - name: Get npm cache directory
        id: npm-cache-dir
        run: echo "dir=$(npm config get cache)" >> $GITHUB_OUTPUT
        
      - name: Cache npm dependencies
        uses: actions/cache@v4
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
          npm ci
          npm run build
        
      - name: Deploy to Neocities
        uses: bcomnes/deploy-to-neocities@master
        with:
          api_token: ${{ secrets.NEOCITIES_API_TOKEN }}
          cleanup: false
          dist_dir: dist
        # if dist_dir is within a subfolder, update the filepath to reflect that
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

## Resources

- [Host your own site on Neocities!](https://neocities.org/)
- [Deploy-To-Neocities Docs](https://github.com/bcomnes/deploy-to-neocities)
- [Astro Docs](https://docs.astro.build) / [Discord](https://astro.build/chat)

## ( `ε´ ) Note!

I made this repo to learn more about Astro and Github Actions, and also for my own convenience.  
I'm still a novice at coding, so I'll try to make this better if I can. Outside contributions are also welcome!

If you find this useful, please go star the original [Deploy-To-Neocities](https://github.com/bcomnes/deploy-to-neocities) repo ⭐~
