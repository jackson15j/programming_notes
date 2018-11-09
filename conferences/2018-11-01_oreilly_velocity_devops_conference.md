2018-11-01 O'Reilly Velocity (DeOps) Conference
===============================================

The [O'Reilly: Velocity (EU)] was a DevOps conference that took place in
London. Here were the schedule links:

* [O'Reilly: Velocity 2018-11-01 Schedule].
* [O'Reilly: Velocity 2018-11-02 Schedule].

Tidbits
-------

Random things sayings/thoughts that are here out of context, but seemed
interesting at the time:

* Sustainable DC (Data Centres) that are carbon neutral:
    * AWS regions: Dublin, Frankfurt, Canada, Oregon.
* _"Middle Out"_ - change people above/below you, to affect change in a
  business.
* Learn from people that area a few steps ahead of you.
* Researching a new area: Thorough investigation + meticulous questioning.
    * Aim for saturation.
* Chaos Testing - Don't forget _retry_ strategies.
* DR (Disaster Recovery) plan.
    * Hold a group meeting and go through scenarios on paper.
    * Thought Exercise between groups for disasters.
* _"5x Hows?"_ vs. _"5x Whys?"_ - _"Whys"_ can lead to blame culture.
* Abstract technical intricacies.
* Metrics - Can you action from reporting??
* Work on User perceivable latency's.
* DevOps is everybodies problem.
* Encrypt Everything Everywhere.
* _Leading edge tech_ requires _comfort with change_.

Books
-----

Book suggestions given:

* [Amazon: Docker on Windows (Elton Stoneman)].
* [SafariBooks: Distributed Systems Observability (Cindy Sridharan)].

Link Dump
---------

Link suggestions given:

---

Talks
=====

[Things you can't cloud your way out of (Jessica Brown, Fastly)]
----------------------------------------------------------------

Fastly are a global Cloud provider that aim to make life easier for customers
to spin up applications on. Okay talk, but I don't need to re-watch. They're
blog may be interesting if you are more hardware/rack biased.

* Issues:
    * POPs (Point of Presence).
    * Auto scale instances issues:
        * Getting hardware (if bespoke) = cost/planning scale.
    * Load Balancer issues:
        * DNS TTL (Time To Live) timeouts.
    * Managing apps across lots of nodes issues.
        * CHR (Cache Hit Ratio) - Percentage increases = High costs for
          customers.
* Solutions:
    * Load Balancers: Use `faild` (in-house) load balancer on their switches.
        * Handles _"drain"_ scenarios.
        * See: [Fastly: Balancing Requets].
    * Managing Apps:
        * Isolation of _"Canaries"_.
* Get your deployment working well:
    * Easy to spin up for Canaries / DR / dev / test...
    * Roll out / Upgrades, should be _"unnoticed"_.
* Fastly still need to care about Hardware/DCs.
* Abstract technical intricacies.
* Inter/Intra-team communications.


[Advanced Docker image build patterns (Gareth Rushgrove, Docker)]
-----------------------------------------------------------------

Fast, but very good talk on the new experimental docker builder. Definitely
worth a watch if possible!

* Have a `root` folder to store your file system, hen `COPY` that into your
  docker container.
    * Benefit: contained file system.
* `HOCON` = environtment variables as first class config citizens.
    * usually Scala only.
* Can now `COPY` from another image. eg.
    * `COPY --from=linuxkit/ca-certificates / /`.

Multibuild Stages:

* Multiple `FROM`'s and `COPY --from` previous containers. eg.
    * Stage1 = build tools.
    * Stage2 = Compile app.
* Can deprecate bespoke `Makefiles` or build scripts, now that multi stage
  logic is in the `Dockerfile`.
* Use aliases to re-use containers. eg. `FROM alpine:3.8 AS alpine`.
* Traditionally serial builds in a `Dockerfile`.
    * Experimental docker build will do things concurrently.
        * Also skips unused stages.
    * Can use this to cache dependencies.
    * Debug a stage: `docker build --target <alias> -t debug .`.
    * Can have test/dev/release aliases for images.
        * Can have `ENTRYPOINT` under the alias.
        * Build the artefact once (Release stage) and copy into the dev/test
          stages.

Links:

* [Github: wagoodman/dive] - Explore layers in a docker image. eg.
    * `docker run --rm -it -v /var/run/docker.sock:/var/run/docker.sock
      wagoodman/dive exeproxy/proxy`.
* [Github: hadolint/hadolint] - `dockerfile` linter. eg.
    * Windows: `Get-Content \Dockerfile | docker run --rm -i hadolint/hadolint`
    * Linux: `cat Dockerfile | docker run --rm -i hadolint/hadolint`
* [Github: GoogleContainerTools/container-structure-test] - Verify structure of
  container based on Company policy config file.
* [Snyk: Snyk for Docker] - Displays vulnerabilities with dependencies. eg.
    * `snyk test --docker alpine`.
* [Github: garethr/multi-stage-build-example] - Talk givers example.
* [Docker: Multistage Build].

Suggested `Dockerfile` workflow:

* BUILD - dependencies for Release.
* RELEASE - `COPY` in code & build.
* DEV/TEST - dev dependencies on top of RELEASE image.
    * `COPY` in code again (avoid cache misses).
* Then `CMD` calls in new aliases that build off RELEASE/DEV/TEST.
* Could use a `Makefile` to drive the different docker stages.
    * `make <build,dev,test,release,doc,check(lint)>`.
* See: [Github: garethr/multi-stage-build-example].

Summary:

* Config as Code.
* Share patterns & practices.
* Maintenance across projects.
* Do less.


[O'Reilly: Velocity (EU)]: https://conferences.oreilly.com/velocity/vl-eu
[O'Reilly: Velocity 2018-11-01 Schedule]: https://conferences.oreilly.com/velocity/vl-eu/schedule/2018-11-01
[O'Reilly: Velocity 2018-11-02 Schedule]: https://conferences.oreilly.com/velocity/vl-eu/schedule/2018-11-02


[Amazon: Docker on Windows (Elton Stoneman)]: https://www.amazon.co.uk/Docker-Windows-101-Production-ebook/dp/B0711Y4J9K
[SafariBooks: Distributed Systems Observability (Cindy Sridharan)]: https://www.oreilly.com/library/view/distributed-systems-observability/9781492033431/


[Things you can't cloud your way out of (Jessica Brown, Fastly)]: https://conferences.oreilly.com/velocity/vl-eu/public/schedule/detail/72252
[Fastly: Balancing Requets]: https://www.fastly.com/blog/building-and-scaling-fastly-network-part-2-balancing-requests
[Advanced Docker image build patterns (Gareth Rushgrove, Docker)]: https://conferences.oreilly.com/velocity/vl-eu/public/schedule/detail/72411
[Github: wagoodman/dive]: https://github.com/wagoodman/dive
[Github: hadolint/hadolint]: https://github.com/hadolint/hadolint
[Github: GoogleContainerTools/container-structure-test]: https://github.com/GoogleContainerTools/container-structure-test
[Snyk: Snyk for Docker]: https://snyk.io/docs/snyk-for-docker
[Docker: Multistage Build]: https://docs.docker.com/develop/develop-images/multistage-build/
[Github: garethr/multi-stage-build-example]: https://github.com/garethr/multi-stage-build-example
