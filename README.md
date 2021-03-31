# automation-experiments

## [Install Gitlab runner](https://docs.gitlab.com/runner/install/osx.html)

```bash
sudo curl --output /usr/local/bin/gitlab-runner "https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-darwin-amd64"
sudo chmod +x /usr/local/bin/gitlab-runner
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