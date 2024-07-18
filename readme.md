# Curriculum Platform

## What is it?

This is a white label multi-tenant platform for us and our code school friends to use to manage our curricula. It's built on top of [Hugo](https://gohugo.io/) and [Netlify CMS](https://www.netlifycms.org/).

The platform, layout, styles and components are all developed in the Hugo module [/common-theme](/common-theme).

The shared content is developed in the Hugo module [/common-content](/common-content). This content is all headless blocks. It doesn't create any pages on your site unless you call it somewhere.

Multi-language support is provided by [Hugo's i18n support](https://gohugo.io/content-management/multilingual/).

Each org builds its own Hugo site that uses the common theme and content modules, and then makes any customisations they need and deploys it wherever they want.

## Examples

- [CodeYourFuture](https://github.com/CodeYourFuture/curriculum) => [https://curriculum.codeyourfuture.io/](https://curriculum.codeyourfuture.io/)
- [MigraCode](./) => [https://curriculum.codeyourfuture.io/](https://curriculum.codeyourfuture.io/)

## To customise this site's look or content

To customise the css, make a dir `assets/custom-theme` and throw any scss in there. It will be compiled and added last.

To add site logo/s, make a dir and add svgs to `assets/custom-images/site-logo/`. They will be added to the site header.

Add your content to `content/` and customise the site config in `config.toml`. Please contribute any improvements you make back to the common theme and content modules.

Anything that is completely unique to your site should be added to this site module, not the common modules, so local /blocks and /layouts are fine.

For each module you import, add a `replace` directive to your `go.mod` file - if you forget to do this, you won't get live updates to your site when shared content changes. CI will remind you if you forget. If you don't need this feature, you can just pull updates manually with `hugo mod get -u`.

## To run on local

## Tools

- [Hugo](https://gohugo.io/) - Static site generator
- [Hugo Pipes](https://gohugo.io/hugo-pipes/introduction/) - Asset pipeline
- [Netlify](https://www.netlify.com/) - Hosting and deployment
- [GitHub](https://github.com/CodeYourFuture/CYF-Signposts) - Source code repository
- [Bash](https://www.gnu.org/software/bash/) - Bash script to create new module structures

## Developing the source website

### To install

```bash
brew install hugo
```

### To run locally

#### Generate a token

You'll need to get a fine-grained GitHub API token which allows read-only access to all public CYF repos from [this page](https://github.com/settings/tokens?type=beta).

Click "Generate new token", enter a token name (can be anything), and how long you want the token to last (if you're doing a one-off contribution, pick a short value; if you're going to be a regular contributor, maybe a longer value).

Make sure the Resource owner is _your_ account if you have a choice.

The "Repository access" you need is "Public repositories (read-only)", and you don't need any account permissions:

<details>
<summary>Open to view screenshot of the required permissions</summary>

![screenshot of required permissions](./readme_repository_access.png)

</details>

#### Set up `.env`

Copy the `.env.example` file over and name it `.env`. Edit the file and then change the line that says `HUGO_CURRICULUM_GITHUB_BEARER_TOKEN` to contain the token that you have generated earlier.

#### Run this command

Run this command in a terminal, substituting in the github API token you generated above:

```bash
npm i
npm run start:dev
```

If you find the build is very slow you can add development options to config/development.toml to speed it up. For example, you can set `disableFastRender = true` to disable fast render, or cache JSON files with `maxAge = 12h`. Don't do these things in production.

```bash
npm run start:dev
```

### To create a new module

```bash
./create_module <module-name>
```

### Linting

All CYF repos use the Prettier standard. However, Prettier doesn't have golang by default so you must install a plugin for it to work in VSCode. I've included it in the package.json.

### Site Search

PageFind runs the search. https://pagefind.app/
It's in the build command on Netlify `hugo && npx pagefind --source "public"`
If you need to develop on this locally, run:

```zsh
rm -rf public &&
hugo && npx pagefind --site "public" --serve
```

And go to http://localhost:1414/ to see the PageFind-served site with search enabled; but there is no hot reload. You can run hugo on http://localhost:1313/ at the same time.
