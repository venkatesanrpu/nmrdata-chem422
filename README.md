Please check the userguide to create your own series of exercises:

[https://docs.nmrium.org/teaching/teaching](https://docs.nmrium.org/teaching/teaching)

This text may be replaced by the description of the exercise.

<-- LINKS -->


<-- ORIGINAL deploy.yml code --->
name: gh-pages deployment

on:
  push:
    branches: main
  workflow_dispatch:

jobs:
  ghpages:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v1
      - name: Use Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20
      - name: install nmrium and create toc
        run: |
          npm i nmrium-cli --global
          nmrium deleteJSONs
          nmrium createExercisesTOC -i true -s true
          nmrium createExercisesCorrection
          nmrium deleteStructures
          nmrium appendLinks
          git status
          git remote -v
      - name: push to gh-pages
        uses: JamesIves/github-pages-deploy-action@v4
        with:
          folder: .
          single-commit: true
