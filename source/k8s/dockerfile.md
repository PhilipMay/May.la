# Dockerfile

## Security
- [Bitnami: Why non-root containers are important for security](https://engineering.bitnami.com/articles/why-non-root-containers-are-important-for-security.html)

## Tricks
- print all executed commands: `set -x` - disable with `set +x` - see https://stackoverflow.com/a/36273740/271118
- do not aburt when single command fails, append this: `|| true` - see https://unix.stackexchange.com/a/325727/454800
