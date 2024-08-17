---
tags:
  - development
  - bash
  - setup
---

# Root script for microservices based projects


## setup.sh

```bash
#!/usr/bin/env bash

set -o errexit
# ensures that the running directory is the one where the script is located
cd "$(dirname "$0")"

GIT_HOST=TODO

function cloneIfNotExists() {
    local repo="$1"
    if [ ! -d "$repo" ]; then
        echo ""
        echo ""
        echo "--------------------------------------------------------"
        echo "Cloning $repo"
        echo "--------------------------------------------------------"
        echo ""

        local remoteUrl="https://$GIT_HOST/$repo.git"
        git clone "$remoteUrl"

        pushd "$repo"

        local targetBranch="develop"
        local targetBranchFound
        targetBranchFound="$(echo "$(git ls-remote --heads "$remoteUrl" "refs/heads/$targetBranch" | grep -c "$targetBranch")")"
        if [ "$targetBranchFound" == 1 ]; then
            git checkout $targetBranch
        fi

        popd
    fi
}


# please keep the alphabetical order of the repositories,
# it's easier to find the missing ones
repositories=(
    auth-ms
    invoice-ms
    shared-lib
    user-ms
)

for REPO in "${repositories[@]}"; do
    cloneIfNotExists "$REPO"
done



# run setup script of the cloned repositories if present
for REPO in */; do
    if [ -d "$REPO" ] && [ -f "$REPO/setup.sh" ]; then
        pushd "$REPO"
        echo ""
        echo ""
        echo "--------------------------------------------------------"
        echo "Running setup.sh in $REPO"
        echo "--------------------------------------------------------"
        echo ""
        ./setup.sh
        popd
    fi
done
```


## .gitignore

```.gitignore
# Ignore everything
*

# But not these files
!.gitignore
!.editorconfig
!mvnw
!mvnw.cmd
!pom.xml
!readme.md
!setup.sh

!.mvn
!.mvn/wrapper
!.mvn/wrapper/maven-wrapper.jar
!.mvn/wrapper/maven-wrapper.properties
!.mvn/settings.xml
```
