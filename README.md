# 🐱‍🚀 Astro + Neocities
*This is a Fork of [Deploy-To-Neocities](https://github.com/bcomnes/deploy-to-neocities) (courtesy of [bcomnes](https://github.com/bcomnes)) ♥ If you are not using an SSG, their method may be what you're looking for.*

## Quickstart

Clone this repo and install the dependencies.

```
cd YOUR_PROJECT_FOLDER
npm install
```

**or**

import the `.github/workflows` folder into a static site generator of your choosing (or just plain ol' html!).

## Usage

```
name: Deploy to Neocities

# only run on changes to master
on:
  push:
    branches:
      - master # change to your branch name if different

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
          node-version: /lts*

      - name: Install dependencies and build site
        run: |
          npm install
          npm run build

      # When the dist folder is ready, deploy it to neocities
      - name: Deploy to Neocities
        uses: bcomnes/deploy-to-neocities@v1
        with:
          api_token: ${{ secrets.NEOCITIES_API_TOKEN }}
          cleanup: true # change to false to prevent orphan files from being removed on neocities
          dist_dir: dist

      #- name: Push changes to GitHub
      #  run: |
      #    git config user.email "YOUR_EMAIL_HERE"
      #    git config user.name "YOUR_USERNAME_HERE"
      #    git add .
      #    git commit --allow-empty -m "actions: deploy + commit"
      #    git push

      ## uncomment this block if you want to push commits to neocities *and* github
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

---

(Optional) Enable permissions for Github Actions to write to your repo _in addition_ to deploying to Neocities.

```
Settings > Actions > General
```

- Scroll down to the `Workflow permissions` and tick the box `Read and Write Permissions`.

---

## Inputs

`api_token`: (REQUIRED): The API token for your Neocities website to deploy to.

`dist_dir`: The directory deployed to Neocities, and Astro's build directory. Do not change this unless you also change Astro's build path.  
Default: `dist`

`cleanup`: If true, `deploy-to-neocities` will delete files on Neocities not found in `dist` or `public`.  
Default: `false`

`protected_files`: An optional glob string used to mark files as protected. Protected files are never cleaned up. Test this option out with cleanup set to false before relying on it. Protected files can still be updated.

## Project Structure

```
/
├── public/
│   └── favicon.svg
├── src/
│   ├── components/
│   │   └── Card.astro
│   ├── layouts/
│   │   └── Layout.astro
│   └── pages/
│       └── index.astro
└── package.json
```

Astro looks for `.astro` or `.md` files in the `src/pages/` directory. Each page is exposed as a route based on its file name.

`src/components/` is for Astro/React/Vue/Svelte/Preact components.

Any static assets, like images, can be placed in the `public/` directory. Learn more [here](https://docs.astro.build/en/core-concepts/project-structure/).

## Commands

All commands are run from the root of the project, from a terminal:

| Command                | Action                                           |
| :--------------------- | :----------------------------------------------- |
| `npm install`          | Installs dependencies                            |
| `npm run dev`          | Starts local dev server at `localhost:3000`      |
| `npm run build`        | Build your production site to `./dist/`          |
| `npm run preview`      | Preview your build locally, before deploying     |
| `npm run astro ...`    | Run CLI commands like `astro add`, `astro check` |
| `npm run astro --help` | Get help using the Astro CLI                     |

## Resources

- [Host your own site on Neocities!](https://neocities.org/)
- [Deploy-To-Neocities Docs](https://github.com/bcomnes/deploy-to-neocities)
- [Astro Docs](https://docs.astro.build) / [Discord](https://astro.build/chat)

## ( `ε´ ) Note!

I made this repo to learn more about Astro and Github Actions, and also for my own convenience.  
While I'm still a novice at coding, maybe others can find it useful too.

If you do, please go star the original [Deploy-To-Neocities](https://github.com/bcomnes/deploy-to-neocities) page! ⭐
