---
tags:
  - development
  - node
  - npm
---

# Useful npm commands


## Publish a node library for local testing

```bash
cd ~/projects/node-redis    # go into the package directory
npm link                    # creates global link
cd ~/projects/node-bloggy   # go into some other package directory.
npm link redis              # link-install the package
```


## Test packages without deploying to npm registry

```bash
# create your local registry # I.E. $HOME/Desktop/local-npm
LOCAL_REGISTRY='/path/to/local-registry' # I.E. $HOME/Desktop/local-npm
mkdir "$LOCAL_REGISTRY"

cd '/path/to/your/program'

# package your program
npm pack --pack-destination "$LOCAL_REGISTRY"

# globally install in your system
npm install -g "$LOCAL_REGISTRY/$YOUR_PACKAGE.tgz"
```
