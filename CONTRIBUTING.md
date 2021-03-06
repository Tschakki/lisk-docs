# Lisk Documentation

- [Workflow](#workflow)
  - [Adding new content](#adding-new-content)
  - [Releasing new versions](#releasing-new-versions)
  - [Changes between versions](#changes-between-versions)
- [Style Guidelines](#style-guidelines)
  - [Writing in Markdown](#writing-in-markdown)
    - [Headings](#headings)
    - [Cross-reference Links](#cross-reference-links)
    - [Images](#images)
  - [Document structure](#document-structure)
    - [Introduction page](#introduction-page)
    - [Structuring content](#structuring-content)
  - [Notes](#notes)

## Workflow

This section describes general processes that need to be followed when contributing to the `lisk-docs` repository.

Each product has its own **development branch**, `dev-{product}`. These development branches are a [subtree](https://git-scm.com/book/en/v1/Git-Tools-Subtree-Merging) of the `lisk-docs` repository, which contain only the relevant documentation files of the respective product. These branches contain the latest changes in the documentation of the product, e.g. documentation for unreleased software versions.

The subtrees are created with the following command:

```bash
git subtree split -P <name-of-folder> -b <name-of-new-branch>
```

For each new version of the product that needs updates / changes in the documentation, a corresponding milestone and branch will be created. Each **version branch** is branched from the development branch for the respective product.

E.g. For Lisk Core version 1.1.0:
- The milestone would be: `Core 1.1.0`
- The version branch would be: `dev-core-1.1.0`

**Example:** How to create a new version branch for `2.0.0` of the `lisk-elements` documentation.

```bash
git checkout dev-elements # change to dev-branch
git pull origin dev-elements # pull latest changes
git checkout -b dev-elements-2.0.0 # create version branch
```

The master branch always contains the official state of the Lisk documentation, which should be identical with content in [https://docs.lisk.io](https://docs.lisk.io).

New issues must be labeled after Product, and should be added to a milestone.

### Adding new content

1. **Create an issue:** If the corresponding issue for the content you want to add does not exist yet, please create the issue first. Remember to specify labels and milestone for the issue, as much as you can.
2. Before working on an issue, **make sure the issue is assigned to you**.
3. **Clone** the `lisk-docs` repository from GitHub: `git clone git@github.com:LiskHQ/lisk-docs.git` (in case you haven't done that already).
4. Check out the branch you want to write documentation for: `git checkout dev-{product}-{version}`.
and **pull the latest changes** : `git pull origin dev-{product}-{version}`. Be sure `{version}` matches the version mentioned in the corresponding issue.
5. Check out a new branch from the version branch which will contain all the changes to solve the issue you are working on. **Name the branch** using this pattern: `123-description-of-the-branch`, where `123` is the issue number of the issue you are trying to solve.
6. Make your changes as intended, commit them and **push it back to GitHub**: `git push origin 123-description-of-the-branch`.
7. On GitHub, open a pull request as usual, reference the issue it solves, and add a short summary of the made changes.

### Releasing new versions

When all issues that belong to a milestone are closed, the current version branch is merged into the `dev-` branch.

The `dev-` branch is then tagged with the corresponding version number and the product name: `{product}-{version}`

At release date of the new version, all new content from the development branches is merged into the `master` branch.

**Example:** Merging changes from a development branch into master:

```bash
git checkout master
git merge -s recursive -Xsubtree=lisk-core dev-core
```

The master branch gets a new tag each time new content from the `dev-` branches is merged. The tag is simple date format, e.g. new content got merged into `master` at February the 15th 2018, the tag for master would be `lisk-docs-2018-02-15`.

### Changes between versions

In case changes to the documentation need to be made after an official release of the documentation, create a new branch for updating the content like so:

1. Check out the dev-branch of the respective product, e.g. `dev-core`.

```bash
git checkout dev-core
```

2. Create a branch from the tag of the version to be updated. The name of the branch follows the general pattern: `dev-{product}-{software-version}-{docs-version}`, where `docs-version` depends on the extent of the content change:

- `1.0.1` - For patches, e.g. content corrections
- `1.1.0` - For minor changes, e.g. additional content
- `2.0.0` - For major changes, e.g. content rewrites

For example, when releasing a minor version for adding a new paragraph to `1.0.0` of the `lisk-core` documentation.

```bash
git checkout -b dev-core-1.0.0-1.1.0 core-1.0.0 # create a new branch from the version tag
```

3. Make the changes, push the branch to GitHub, then open a pull request against the dev-branch of the respective product.

4. After the pull request has been approved, merge the changes into the dev-branch.

```bash
git checkout dev-core
git merge dev-core-1.0.0-1.1.0
```

5. Tag the branch with the new documentation version.

```bash
git tag core-1.0.0-1.1.0
```

6. Port the changes to all newer version branches of the documentation where necessary.

```bash
git checkout dev-core-{version}
git merge core-1.0.0-1.1.0
```

7. Finally, merge the changes into the master branch.

```bash
git checkout master
git merge -s recursive -Xsubtree=lisk-core dev-core
```

## Style guidelines

To keep the documentation experience intuitive and consistent for the user, each product documentation needs to follow the common style guidelines for Lisk Documentation.

Please read it carefully and use it as a checklist before and after every participation.

### Writing in markdown

The whole documentation content is written in Markdown.

For reference: [Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)

#### Headings

Headings create automatically internal anchors that can be referenced in other parts of the documentation. Use headings to structure the content of each page.

```
# Main title

## Section 1

### Subsection

## Section 2

[...]
```

#### Cross-reference links

> The cross-reference links can be easily broken. Remember this section when removing or adding pages, sections or headings.

##### When to use references

- In table of contents
- Inside of the content. Scan content for helpful cross-references

##### How to create references

> Use internal / relative links instead of external links where possible.

```
[Link to Section 1](#section-1)
[Link to another docs page](path/to/page.md)
[Link to other Website](https://nodejs.org/en/)

[...]

## Section 1
```

#### Images

> Only include images, if they are informative for the user.

If you want to include a picture on a page, upload the image in the assets folder and use a relative link to the image.

Image name should be: `lisk_PRODUCT-DEFINITION`. Optionally and depending on how the documentation grows, another tag can be added as section ending in `lisk_PRODUCT-SECTION-DEFINITION`

Example:

```
![alt text for lisk logo](lisk_protocol-Logo.svg "Logo Title Text")
```

### Document structure

When to use new pages, sub pages or sections for new content.

#### Introduction page

On root level of each product documentation you find an introduction page for the respective product. This page is always required.

An introduction page should have at least the following sections:

1. **Table of contents:** The introduction should start with a table of contents with relative links to all other existing documentation sites for the respective product.
2. **Product description:** Try to describe the product precisely in 1-2 sentences. Then, elaborate about the general purpose of the product, e.g. highlight use cases and top features.
3. **Codebase reference:** Link to the GitHub repository of the product, and reference contribution guidelines.

#### Structuring content

The following list gives some suggestions how to structure the content:

1. **Parent pages:** A page that contains one or more child pages. It should always start with a table of contents, referencing all existing child pages.
2. **Child pages:** If content is connected, but can stand independent from each other.
3. **Section in page:** Content that is closely connected to the content of the other sections / the main title of the page.

### Notes

If certain content needs to be highlighted or deserves special attention from the reader, use notes as described below.

```
> Only include images, if they are informative for the user.
```
