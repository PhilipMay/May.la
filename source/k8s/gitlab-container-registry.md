# GitLab Container Registry
- see https://docs.gitlab.com/ee/user/packages/container_registry/index.html
- see GitLab CI/CD: https://docs.gitlab.com/ee/ci/index.html
- see `.gitlab-ci.yml` template: https://gitlab.com/gitlab-org/gitlab-foss/-/blob/master/lib/gitlab/ci/templates/Docker.gitlab-ci.yml

## Password Handling
```text
$ docker login -u "$CI_REGISTRY_USER" -p "$CI_REGISTRY_PASSWORD" $CI_REGISTRY
WARNING! Using --password via the CLI is insecure. Use --password-stdin.
WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store
```
