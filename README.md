# PoWDump docs

This repo contains the documentation for the PoWDump project.

The repository is built with docusaurus. To run the docs locally, run:

### Installation

```
$ yarn
```

### Local Development

```
$ yarn start
```

This command starts a local development server and opens up a browser window. Most changes are reflected live without
having to restart the server.

### Build

```
$ yarn build
```

This command generates static content into the `build` directory and can be served using any static contents hosting
service.

### Deployment

There is a github action configured to deploy the docs to github pages. The deployed docs can be found
at: https://docs.dump.today


### To update any of the pages
Down at the bottom of every page on https://docs.dump.today there is an edit link to the corresponding markdown file. The
link will open the docs in the github editor. You can edit the file and create a pull request. Once the pull request is
merged the docs will be auto-updated.

### To add a new page
To add a new page, create a new markdown file in the `docs` folder.