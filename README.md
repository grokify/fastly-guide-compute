# Unoffical Fastly Guide for Compute@Edge

This guide is a companion guide to the officaly Fastly documentation.

Read the Guide here:

[https://grokify.github.io/fastly-guide-compute/](https://grokify.github.io/fastly-guide-compute/)

It covers:

1. [End-to-end "Hello, World!" example](https://grokify.github.io/fastly-guide-compute/compute/quickstart_javascript/)
1. Fastly Management Console information for [creating services](https://grokify.github.io/fastly-guide-compute/basics/service/) and [uploading packages](https://grokify.github.io/fastly-guide-compute/compute/uploading_and_activation/)
1. Day 2 deployment information for [automating packaging](https://grokify.github.io/fastly-guide-compute/compute/packaging/)
2. Day 2 deployment information for [API and SDK upload and activation options](https://grokify.github.io/fastly-guide-compute/compute/uploading_and_activation/)

## Running the Guide Locally (for authors)

This Developer Guide is built on top of Mkdocs, a self-contained documentation server. Writers are encouraged to install Mkdocs locally so that you can edit files and preview your changes before they are pushed to production servers.

```bash
git clone https://github.com/grokify/fastly-guide-compute.git
cd fastly-guide-compute
pip install --upgrade mkdocs
pip install --upgrade mkdocs-material
mkdocs serve
```

Then you should be able to load http://localhost:8000 to view the documentation.

## Deploying the Guide

Once the updates have been made, use the following command to publish to GitHub pages:

```bash
mkdocs gh-deploy
```