# automation-experiments

Builds Spring Boot application, creates OCI image and uploads to Harbor.

## [Install Gitlab runner](https://docs.gitlab.com/runner/install/osx.html)

```bash
sudo curl --output /usr/local/bin/gitlab-runner "https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-darwin-amd64"
sudo chmod +x /usr/local/bin/gitlab-runner
```

## Gitlab Runner configuration (Worked for me on Docker Desktop - Mac OSX)

I also needed to disable TLS by `DOCKER_TLS_CERTDIR: ""`

Contents of `~/.gitlab-runner/config.toml`

```yaml
concurrent = 1
check_interval = 0

[session_server]
  session_timeout = 1800

[[runners]]
  name = "alex-local"
  url = "https://gitlab.eng.vmware.com/"
  token = "my-super-secret-token"
  executor = "docker"
  [runners.custom_build_dir]
  [runners.cache]
    [runners.cache.s3]
    [runners.cache.gcs]
    [runners.cache.azure]
  [runners.docker]
    tls_verify = false
    run_untagged = true
    image = "docker:latest"
    privileged = true
    disable_entrypoint_overwrite = false
    oom_kill_disable = false
    disable_cache = false
    volumes = ["/var/run/docker.sock:/var/run/docker.sock", "/cache"]
    shm_size = 0
    pull_policy = "if-not-present"
```

## [Register runner](https://docs.gitlab.com/runner/register/#macos)

```bash
gitlab-runner register
```

Provide all settings as prompted.

Verify installation.

```bash
gitlab-runner install
```

Start the runner.

```bash
gitlab-runner start
```

Navigate to Gitlab CI/CD Settings and verify that runner is activated

## Environment Variables

Need to enter variables to run the pipeline.

|key   |value   |
|---|---|
|SPRING_ACTIVE_PROFILE   |default   |
|DOCKER_REPO   |harbor-repo.vmware.com/build_service/chachkies   |
|DOCKER_USER   |ashumilov   |
|DOCKER_PASSWORD    |HeyNow :)!   |

## Commands reference

Test OCI build.

```bash
docker build chachkies --build-arg SPRING_ACTIVE_PROFILE=default -t harbor-repo.vmware.com/build_service/chachkies -f chachkies/Dockerfile
```