Docker Notes
============

* `docker run -it -d -p<host:container> --name=<name>
  <image_name:image_version>` - Run up an container, name the instance,
  passthrough ports, detach.
* `docker start <name>` - Start a named container.
* `docker ps` - check running instances.
* `docker exec -it <name> <cmd>` - Execute command inside running instance.
* `docker stop <name>`

Link Dump
=========

* [Github: hadolint/hadolint] - `Dockerfile` linter.
* [Snyk: snyk for docker] - Snyk is a Docker Security scanning tool.
* [Github: garethr/multi-stage-build-example].
* [Docker Docs: Dockerfile reference].


[Github: hadolint/hadolint]: https://github.com/hadolint/hadolint
[Snyk: snyk for docker]: https://snyk.io/docs/snyk-for-docker/
[Github: garethr/multi-stage-build-example]: https://github.com/garethr/multi-stage-build-example
[Docker Docs: Dockerfile reference]: https://docs.docker.com/engine/reference/builder/
