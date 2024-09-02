# Konflux build system Files

This directory holds the requirements.txt file use in the Konflux builds to pre-fetch the required packages.

Here are two files that we need to keep:

- pyproject.toml: keep track of the dependencies needeed to build the `modelmesh-runtime-adapter` Container image
- requirements.txt: is the reseult of the `poetry export -f requirements.txt` command.

## How to update the requirements.txt file

First, make sure that the versions are aligned, it can be check in the [Dockerfile](../Dockefile) file.

Generating the `poetry.lock` file (do not push this file, delete afterward):

```bash
poetry update --lock
```

Then, generate the `requirements.txt` file:

```bash
poetry export -f requirements.txt --output requirements.txt
```

Finally, delete the `poetry.lock` file.
Then commit the changes and push.

# Konflux configuration

Build configuration, it requires the build be configured as follows:

```yaml
- name: prefetch-input
  value: |
    [{"type": "gomod"}, {"type": "rpm"}, {"type": "pip", "path": "konflux-files", "requirements_files": ["requirements.txt"], "allow_binary": "true"}]
```
