# DART Documentation for Users and Developers

This project contains documentation for DART users and developers. Users are people who want to package and ship files using the DART UI or command-line interface. Developers are people who want to write plugins to extend DART's feature set.

This is a source repository. The public-facing documentation lives at https://aptrust.github.io/dart-docs/.

## Local Editing

If you want to clone this respository and edit it locally, you'll need to install (mkdocs)[https://www.mkdocs.org/] and the (material)[https://squidfunk.github.io/mkdocs-material/] theme. We also use (pygments)[http://pygments.org/] for highlighting code samples. Assuming you have reasonably current versions of Python and pip, you can do that with the following commands:

```
pip install mkdocs
pip install mkdocs-material
pip install pygments
```

Once those are installed, you can run the docs locally with the command `mkdocs serve`.

## Deploying to GitHub pages

```
mkdocs gh-deploy
```
