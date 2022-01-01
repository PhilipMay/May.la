# GitLab Container Registry
- see https://docs.gitlab.com/ee/user/packages/container_registry/index.html
- see GitLab CI/CD: https://docs.gitlab.com/ee/ci/index.html
  - Predefined variables reference: https://docs.gitlab.com/ee/ci/variables/predefined_variables.html 
- see `.gitlab-ci.yml` template: https://gitlab.com/gitlab-org/gitlab-foss/-/blob/master/lib/gitlab/ci/templates/Docker.gitlab-ci.yml
- multiple tags: https://stackoverflow.com/a/56905333/271118

## My Test Project
- my test project: https://gitlab.com/PhilipMay/docktest
- run a container: `sudo docker run registry.gitlab.com/philipmay/docktest`

## Password Handling
```text
$ docker login -u "$CI_REGISTRY_USER" -p "$CI_REGISTRY_PASSWORD" $CI_REGISTRY
WARNING! Using --password via the CLI is insecure. Use --password-stdin.
WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store
```

- fixx this with `echo $CI_DEPLOY_PASSWORD | docker login -u $CI_DEPLOY_USER --password-stdin registry.gitlab.com` - see https://stackoverflow.com/a/53415082/271118
- this does not seem to work!
