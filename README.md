# Astro + Neocities

🎈 Because Astro is build around shipping _less_, I thought it would be neat to try out on Neocities.

Astro is a static site builder that renders your entire page to static HTML, removing all JavaScript from the final build by default. Interactive components are hydrated only when they become visible on the page.  
You can build with modern frameworks or just plain ol’ HTML + JavaScript.

## Set-Up

Clone this repository and run the following commands.

```
cd YOUR_FOLDER
npm install
```

In the `.github\workspace` folder, remove `.test` from the end of `neocities.yaml.test`.

## Github Actions

This deployment method relies on **Github Actions** (courtesy of [bcomnes](https://github.com/bcomnes/deploy-to-neocities)) ♥

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
      - run: echo "🎉 Job automatically triggered by ${{ github.event_name }} event."
      - run: echo "🐧 Running on ${{ runner.os }}!"
      - run: echo "🔎 Branch :/ ${{ github.ref }}"
      - run: echo "🔎 Repository:/ ${{ github.repository }}."
      - name: Checkout the repository code
        uses: actions/checkout@v3
      - run: echo "🍏 Job status :/ ${{ job.status }}."
      - name: Install Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '16'

      - name: Install dependencies and build site
        run: |
          npm install
          npm run build

      # When the dist_dir is ready, deploy it to neocities
      - name: Deploy to Neocities
        uses: bcomnes/deploy-to-neocities@v1
        with:
          api_token: ${{ secrets.NEOCITIES_API_TOKEN }}
          cleanup: false # `true` will automatically remove orphan files from neocities
          dist_dir: dist

      #- name: Push changes to GitHub
      #  run: |
      #    git config user.email "YOUR_EMAIL_HERE"
      #    git config user.name "YOUR_USERNAME_HERE"
      #    git add .
      #    git commit --allow-empty -m "actions: deploy + commit"
      #    git push

      ## uncomment this block if you want to push commits to neocities *and* github
      ## see "⚠" section of README for more info
```

Generate your Neocities API token at:

```
https://neocities.org/settings/{{your-sitename}}#api_key
```

Next, go to your Github Repository setting and enter your key as a new secret:  
`Settings > Secrets and variables > Actions > New repository secret`

> Name: NEOCITIES_AP_TOKEN  
> Secret: `ENTER YOUR API KEY HERE`

---

⚠ This step is for pushing changes to code on neocities **AND** your github repo. Otherwise, your repo will not receive any commits. If that's fine with you, feel free to skip this step.

- Enable permissions for the Actions Bot to write to your repo.
- On your repo page, go to `Settings > Actions > General`
- Scroll down to the `Workflow permissions` and tick the box `Read and Write Permissions`.

---

## 🚀 Project Structure

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

## 🧞 Commands

All commands are run from the root of the project, from a terminal:

| Command                | Action                                           |
| :--------------------- | :----------------------------------------------- |
| `npm install`          | Installs dependencies                            |
| `npm run dev`          | Starts local dev server at `localhost:3000`      |
| `npm run build`        | Build your production site to `./dist/`          |
| `npm run preview`      | Preview your build locally, before deploying     |
| `npm run astro ...`    | Run CLI commands like `astro add`, `astro check` |
| `npm run astro --help` | Get help using the Astro CLI                     |

## 👀 Resources

- [Host your own site on Neocities!](https://neocities.org/)
- [Deploy-To-Neocities Docs](https://github.com/bcomnes/deploy-to-neocities)
- [Astro Docs](https://docs.astro.build) / [Discord](https://astro.build/chat)

## ( `ε´ ) Enjoy!

I'm a novice at coding, but I wanted to post something helpful.  
Hopefully you won't run into many issues

### TODO:

..test deploy
