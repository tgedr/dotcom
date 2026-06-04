---
title: bashutils
description: bash scripting utilities
slug: bashutils
date: 2026-06-04 00:00:00+0000
image: cover.jpg
categories:
    - artifacts
tags:
    - artifacts
weight: 1       # You can add weight to some posts to override the default sorting (date descending)
---

Writing production-ready Bash scripts and reuse them across multiple projects ensuring predictable automation behavior across local environments and CI/CD pipelines alike, is really important.

We have developed and have been using [bashutils](https://github.com/tgedr/bashutils) for a while.

How to use it?

download the helper script into the root of your project, with:

```bash
curl -fsSL https://raw.githubusercontent.com/tgedr/bashutils/main/bashutils-template.sh \
    -o ./helper.sh && chmod +x ./helper.sh
```

if eventually you experience issues with the corporation proxy, try (python needed):

```bash
curl -fsSL "https://api.github.com/repos/tgedr/bashutils/contents/bashutils-template.sh" \
| python3 -c "import sys,json,base64; print(base64.b64decode(json.load(sys.stdin)['content']).decode())" \
    > ./helper.sh && chmod +x ./helper.sh
```

have a go, run `./helper.sh`

- if non-existent, it creates the files `.variables` (should be version-managed), `.local_variables` and `.secrets` (these 2 are for personal development purposes and should NOT be version-managed) next to the script
- it downloads `.bashutils` on the first run,m it gets then included from now on
- it provides a set of logging functions
- on later runs it checks for updates at most once per day and replaces the local `.bashutils` from `main` only when newer
- every downloaded `.bashutils` file is verified with SHA256 using `.bashutils.checksum`
- you can now reuse `.bashutils` functions by referencing functions in your own `.helper.sh`:
    ```bash
    case "$1" in
        reqs)
        reqs
        ;;
        verify)
        verify_env
        ;;
        deploy)
        databricks_bundle_deploy "$2" "$3"
        ;;
        destroy)
        databricks_bundle_destroy "$2" "$3"
        ;;
        *)
        usage
        ;;
    esac
    ```
- you can also add your own functions directly to the `.helper.sh` script
- you are encouraged to submit PR's to contribute with new functionality to `.bashutils`

do check it.

