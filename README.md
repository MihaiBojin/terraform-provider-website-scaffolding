# Terraform Provider Website Scaffolding

This is a repository which can act as scaffolding for setting up a new repository for a [terraform provider](https://github.com/terraform-providers).

Some of the source code in this repository was copied over from existing provider repositories such as [terraform-provider-google](https://github.com/terraform-providers/terraform-provider-google).

The commands detailed below work on *MacOS* and *Linux*. On Windows, you can follow the same general steps, but will have to run the commands manually.


## Tutorial: Setting up a new terraform provider repository

1\. Prerequisites

Note: Documenting how to install prerequisites is not in the scope of this tutorial.

- Go: Please consult [Golang's installation instructions](https://golang.org/doc/install).
- Docker: [Getting started](https://www.docker.com/get-started)

2\. Clone `terraform-website`

```bash
git clone https://github.com/hashicorp/terraform-website \
    "$GOPATH/src/github.com/hashicorp/terraform-website"
```

3\. Clone this repo

To simplify running the following steps, execute the following command in your shell:

`export PROVIDER_REPO=desired-name` (where _desired-name_ should be the name of your new provider).

```bash
mkdir -p "$GOPATH/src/github.com" && cd "$GOPATH/src/github.com"
git clone git@github.com:MihaiBojin/terraform-provider-website-scaffolding.git \
    "terraform-provider-$PROVIDER_REPO"
```

4\. Update the name of the repository

Edit [GNUmakefile](./GNUmakefile) and change the following line:

```bash
# Change
PKG_NAME=website-scaffolding

# to
PKG_NAME=desired-name
```

5\. Rename the layout file

```bash
pushd "$GOPATH/src/github.com/terraform-provider-$PROVIDER_REPO/website"
mv "website-scaffolding.erb" "$PROVIDER_REPO.erb"
popd
```

6\. Set-up the required symlinks

```bash

# link the new provider repo
pushd "$GOPATH/src/github.com/hashicorp/terraform-website/ext/providers"
ln -sf "$GOPATH/src/github.com/terraform-provider-$PROVIDER_REPO" "$PROVIDER_REPO"
popd

# link the layout file
pushd "$GOPATH/src/github.com/hashicorp/terraform-website/content/source/layouts"
ln -sf "../../../ext/providers/$PROVIDER_REPO/website/$PROVIDER_REPO.erb" "$PROVIDER_REPO.erb"
popd

# link the content
pushd "$GOPATH/src/github.com/hashicorp/terraform-website/content/source/docs/providers"
ln -sf "../../../../ext/providers/$PROVIDER_REPO/website/docs" "$PROVIDER_REPO"
popd

# start middleman
cd "$GOPATH/src/github.com/terraform-provider-$PROVIDER_REPO"
make website
```

6\. View the changes

```
# MacOS
open "http://localhost:4567/docs/providers/$PROVIDER_REPO"

# Linux 
sensible-browser "http://localhost:4567/docs/providers/$PROVIDER_REPO"
```

7\. Done

You are ready to write docs for your new provider repository.

You can start by updating all references to `website-scaffolding` from files under [website](./website/) to reflect your repository's name.

If you'd like to update these instructions (either to add new features or to fix any issues), please feel free to open a Pull Request.

**Enjoy!**
