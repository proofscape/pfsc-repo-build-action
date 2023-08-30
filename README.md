# Proofscape repo build action `v2`

This action builds a Proofscape content repo. It can be used in a workflow, for
example to check on pushes and pull-requests that the repo at least builds.

## Usage

```yaml
- uses: proofscape/pfsc-repo-build-action@v2
  with:
    # The owner/reponame of the Proofscape content repo to be built.
    # Default: ${{ github.repository }}
    content-repo: ''

    # The version at which to build the Proofscape content repo.
    # If unspecified, build @WIP, meaning the reference or SHA that triggered
    # the workflow, or the default branch if no such event.
    # Default: WIP
    content-vers: ''
    
    # Whether to do a clean build, ensuring all modules are re-read
    # from source.
    # Default: false
    clean: ''

    # PISE version to use when building. This is the tag for the
    # `proofscape/pise` docker image we use to perform the build.
    # Must be `0.28.0` or later. To use earlier versions of PISE, must use
    # `v1` of this `pfsc-repo-build-action`.
    # Default: latest
    pise-vers: ''

    # Working directory, i.e. space in which to do things like checkout the
    # content repo. The latter will be checked out under `./proofscape/lib/gh`
    # relative to this directory.
    # Default: ${{ github.workspace }}
    workspace: ''

    # Name of docker volume to use for the lib directory. After the build
    # completes (if successful), this volume will be populated with the
    # contents of `./proofscape/lib`. Can be useful in later steps, if desired.
    # Default: pfsc-lib
    lib-volume: ''

    # Name of docker volume to use for the build directory. After the build
    # completes (if successful), this volume will be populated with the
    # contents of `./proofscape/build`. Can be useful in later steps, if desired.
    # Default: pfsc-build
    build-volume: ''

    # Name of docker volume to use for the graphdb directory. After the build
    # completes (if successful), this volume will be populated with the
    # contents of `./proofscape/graphdb`. Can be useful in later steps, if desired.
    # Default: pfsc-gdb
    gdb-volume: ''

    # If "yes", keep the PISE container running after completing.
    # Default: no
    container-persist: ''

    # The name of the PISE docker container in which the build takes place.
    # Default: pise
    container-name: ''

    # Controls diagnostic log output
    # Default: 1
    debug-level: ''
```

# Scenarios

- [Build at WIP](#Build-at-WIP)
- [Build at a numbered version](#Build-at-a-numbered-version)
- [Examine the build results](#Examine-the-build-results)

## Build at WIP

```yaml
- uses: proofscape/pfsc-repo-build-action@v2
```

## Build at a numbered version

```yaml
- uses: proofscape/pfsc-repo-build-action@v2
  with:
    content-vers: v3.1.4
```

## Examine the build results

```yaml
- uses: proofscape/pfsc-repo-build-action@v2
  with:
    container-persist: yes
- working-directory: proofscape/build/gh/${{ github.repository }}/WIP
  run: |
    docker exec pise bash -c "\
      grep 'some expected string' path/to/some/module/Thm.dg.json \
    "
```
