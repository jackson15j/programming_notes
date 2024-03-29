Cloud Notes
===========

* [12 Factor] principles.

[Principles of Chaos Engineering]
---------------------------------

* [Principles of Chaos Engineering] covers experimenting on a deployment/system
  to uncover systemic issues:
    * Define a measurable _"steady state"_.
    * Expectation that _"steady state"_ should continue in both control/test
      groups.
    * Introduce _"real world"_ events (crashes, connectivity failures,
      infrastructure failures,...).
    * Disprove expectations.
* Confidence comes from extremes it takes to disprove the control vs test
  expectation,

[ChaosMonkey]
-------------

* See:
    * [Gihub: chaosmonkey].
    * [Netflix: OSS].
* Verify your stateless/statefull cloud application/infrastructure deployments
  resilience by randomly nuking nodes.

Docker/Kubernetes
-----------------

Docker:

* [Docker: Dockerfile reference].
* [Github: garethr/multi-stage-build-example]. See: [Velocity: Advanced Docker
  image build patterns (Gareth Rushgrove, Docker)].

Kubernetes:

* [Kubernetes Basics].
* [Kubernetes Deployment].
* [Kubernetes Service].

AWS
===

ASW CLI
-------

* [AWS CLI docker].
* `docker run --rm -it -v ~/.aws:/root/.aws  amazon/aws-cli --color=on --profile=development-poweruser --region=us-east-1 ec2 describe-instances`


Latest AMI's
------------

AMI's are rapidly updated, so here are the commands to get back the latest
Linux/Windows AMI paths, which can then be used to get the latest ID for a
specific path:

```bash
# All latest Amazon Linux AMI's:
aws ssm get-parameters-by-path --path "/aws/service/ami-amazon-linux-latest" --region <region> --profile <profile>
# All latest Windows AMI's:
aws ssm get-parameters-by-path --path "/aws/service/ami-windows-latest" --region <region> --profile <profile>
```

SSM
---

* Run PowerShell command and log to CloudWatch:

  ```bash
  aws ssm send-command --instance-ids "<instance_id>" --profile <profile> --cloud-watch-output-config "CloudWatchLogGroupName=echotest,CloudWatchOutputEnabled=true" --document-name AWS-RunPowerShellScript --parameters commands="echo Hello World"
  ```

INVESTIGATE
===========

Link dump of things I need to **INVESTIGATE** or document further:

* [GCloud: Pusing Docker images].
* [GCloud: JSON Key Authentication].
* [GCloud: Configuring Domain Names & Static IPs].
* [Labels & Selectors].
* [Github: CloudNativeWales/container.training].
* [Google: SRE (Site Reliability Engineering) Handbook].
* [NovemberFive: PyPI repo on AWS S3].
* [Zipkin] - Distributed tracing system to diagnose latency issues within
  microservice architectures.
* [Loader.io] - Free load tester for Web App API's.


[12 Factor]: https://12factor.net
[Principles of Chaos Engineering]: http://principlesofchaos.org

[ChaosMonkey]: https://netflix.github.io/chaosmonkey/
[Gihub: chaosmonkey]: https://github.com/netflix/chaosmonkey
[Netflix: OSS]: https://netflix.github.io

[AWS CLI docker]: https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-docker.html

[GCloud: Pusing Docker images]: https://cloud.google.com/container-registry/docs/pushing-and-pulling
[GCloud: JSON Key Authentication]: https://cloud.google.com/container-registry/docs/advanced-authentication#using_a_json_key_file
[GCloud: Configuring Domain Names & Static IPs]: https://cloud.google.com/kubernetes-engine/docs/tutorials/configuring-domain-name-static-ip#step_2a_using_a_service
[Kubernetes Basics]: https://kubernetes.io/docs/tutorials/kubernetes-basics/
[Kubernetes Deployment]: https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
[Kubernetes Service]: https://kubernetes.io/docs/concepts/services-networking/service/
[Labels & Selectors]: https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/
[Github: CloudNativeWales/container.training]: https://github.com/cloudnativewales/container.training
[Google: SRE (Site Reliability Engineering) Handbook]: https://landing.google.com/sre/sre-book/toc/
[NovemberFive: PyPI repo on AWS S3]: https://novemberfive.co/blog/opensource-pypi-package-repository-tutorial/
[Docker: Dockerfile reference]: https://docs.docker.com/engine/reference/builder/
[Github: garethr/multi-stage-build-example]: https://github.com/garethr/multi-stage-build-example
[Velocity: Advanced Docker image build patterns (Gareth Rushgrove, Docker)]: conferences/2018-11-01_oreilly_velocity_devops_conference.md#advanced-docker-image-build-patterns-gareth-rushgrove-docker
[Zipkin]: https://zipkin.io
[Loader.io]: https://loader.io
