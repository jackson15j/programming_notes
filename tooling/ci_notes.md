CI Notes
========

[Travis]
--------

* [StackOverflow: How to run Travis CI locally].
* [Travis: Troubuleshoot locally in a docker image].
* [Docker: travisci].

[Gitlab]
--------

Configuring CI the Gitlab way:

* Create a `.gitlab-ci.yml` in the root directory (See: [Gitlab: CI Quick
  Start] and [Gitlab: Yaml (config)]), with a definition on how to build the
  project.
* Register a Gitlab-Runner docker container (See: [Gitlab: Register
  gitlab-runner]):

  ```bash
  docker run --rm -t -i -v /path/to/gitlab-runner-config:/etc/gitlab-runner --name gitlab-runner gitlab/gitlab-runner register \
     --non-interactive \
     --executor "docker" \
     --docker-image docker:stable \
     --url "https://<gitlab_url>/" \
     --registration-token "<gitlab_pipeline_token>" \
     --description "<description_for_this_runner>" \
     --tag-list "<tags>" \
     --run-untagged \
     --locked="false"
  ```

* **Note:** `Token` and `URL` are found in Gitlab under: Project > Settings >
  CI / CD > Runners > Expand > Set up a specific Runner manually.
* **Note:** If `--run-untagged` is missing, or the interactive register steps
  are taken, the runner will **only** run jobs for the tags it knows.
* Manually update the `volumes` field (there is no way to do this during
  register) in `/path/to/gitlab-runner-config/config.toml` as follows:

  ```bash
  volumes = [ "/var/run/docker.sock:/var/run/docker.sock", "/cache"]
  ```

* Run the Gitlab-Runner container (See: [Gitlab: Run docker gitlab-runner]):

  ```bash
  docker run -d --name gitlab-runner --restart always \
    -v /path/to/gitlab-runner-config:/etc/gitlab-runner \
    -v /var/run/docker.sock:/var/run/docker.sock  \
    gitlab/gitlab-runner:latest --debug run
  ```

* To check logs of the Gitlab-Runner:

  ```bash
  docker logs -ft gitlab-runner
  ```

* Gitlab should be using your runner for all pending jobs now.
* Also see:
    * [Gitlab: Building Docker Images with Gitlab CI/CD].
    * [Gitlab: Using docker Images].
    * [Gitlab: Docker Executors].
    * [Docker: docker (DinD: Docker in Docker)].

[Jenkins]
---------

* [Jenkins: Declarative Pipeline Syntax].
* Get all _"Keep Forever"_ builds for a job:

  ```
  <jenkins_url>/job/<job_name>/api/xml?depth=2&xpath=//build[keepLog=%22true%22]/number&wrapper=forever
  ```


[Travis]: https://travis-ci.com
[StackOverflow: How to run Travis CI locally]: https://stackoverflow.com/questions/21053657/how-to-run-travis-ci-locally#35972902
[Travis: Troubuleshoot locally in a docker image]: https://docs.travis-ci.com/user/common-build-problems/#Troubleshooting-Locally-in-a-Docker-Image
[Docker: travisci]: https://hub.docker.com/r/travisci/

[Gitlab]: https://about.gitlab.com
[Gitlab: CI Quick Start]: https://docs.gitlab.com/ee/ci/quick_start/
[Gitlab: Yaml (config)]: https://docs.gitlab.com/ee/ci/yaml/
[Gitlab: Register gitlab-runner]: https://docs.gitlab.com/runner/register/index.html#one-line-registration-command
[Gitlab: Run docker gitlab-runner]: https://docs.gitlab.com/runner/install/docker.html#docker-image-installation-and-configuration
[Gitlab: Building Docker Images with Gitlab CI/CD]: https://docs.gitlab.com/ee/ci/docker/using_docker_build.html
[Gitlab: Using docker Images]: https://docs.gitlab.com/ee/ci/docker/using_docker_images.html
[Gitlab: Docker Executors]: https://docs.gitlab.com/runner/executors/docker.html
[Docker: docker (DinD: Docker in Docker)]: https://hub.docker.com/_/docker/

[Jenkins]: https://jenkins.io
[Jenkins: Declarative Pipeline Syntax]: https://jenkins.io/doc/book/pipeline/syntax/
