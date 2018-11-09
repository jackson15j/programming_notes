2018-11-08 Cloud Native Wales
=============================

* See: [Meetup: Cloud Native Wales event] for posted agenda.
* DevOps Group:
    * Expanded to London.
    * Jobs in Cardiff/London offices.

Nat Morris (Netflix)
--------------------

Very good talk and interesting how their culture defines how they dev/deploy so
freely via CI.

* Senior Dev on Content Delivery team.
* Based in Fishguard, Wales (remote worker since 2013).
* Used to work at Cumulus Network.
* _"[Netflix Culture]"_.
    * You can deploy whatever you want but must maintain & scale it!!
    * You can choose your own language for your service.
    * Deploy is so fast, so expect failure and fix (assume: fix forward or
      rollback).
    * Highly aligned, loosely coupled.
* Scale:
    * > 1/3 of internet traffic at peak time.
    * More traffic into AWS than out due to metrics being sent in.
    * 152,000 EC2 instances at peak (mainly across 3 regions).
    * 500 services.
    * 680 Casandra clusters (400 billion events a day).
    * 730 Elastic Search clusters.
    * 40 Petabytes in S3.
    * 510 Jenkins slaves.
    * 10,000 start/sec events (_"Play"_ button presses).
    * 900 Devs, no Ops (well, 2 that manager the PagerDuty system).
* Client Devices:
    * Thin `C` app on devices which hosts a Java app.
    * Netflix can push down app changes without engaging the (slow) vendor
      certification process to do updates (only engage for C layer updates).
* Services:
    * Was/now: REST, now/future: gRPC.
    * Was/now: EC2 containers, now/future: Docker.
        * Docker on EC2 (before ECS was a thing).
        * AWS ECS is not quite right for them yet.
        * 3 million containers launched a week.
        * Looks like an EC2 instance (IAM roles, IP per container, ALB...).
* CI/CD:
    * Stash > Jenkins > Artifactory/Spiniker > Deploy.
    * 15mins (Consider slow) deploy to EC2.
    * Promote builds instead of rebuild (avoid issues with different
      dependencies between test/staging/production stages).
    * 2m30s deploy of docker on EC2 (If naughty and push immediately from test
      to production).
* Chaos Processes:
    * [Netflix: Simian Army].
    * [Chaos Monkey] - All services default Opt-in. Typically spin up more
      containers behind an ALB for protection.
    * [Latency Monkey] - REST delays, gRPC soon.
    * [Chaos Gorilla] - AWS availability zone down.
    * [Chaos King] - Kill an entire region.
    * `null` check on gRPC calls!
* Control Plane:
    * UI layer that talks to AWS.
* Data Plane:
    * Content delivery when you push _"Play"_
    * Built their own CDN (Content Delivery Network), due to slowness of AWS.
        * Geolocated content servers.
        * Give list of URL's that your player client picks from.
    * Client device receives 30secs of HTTP chunks, that will be used in 2mins
      time.
* Servers:
    * 2U custom _"red"_ servers (Normal content).
        * Boots FreeBSD off 2x SSD's.
        * Nginx.
        * 150TB's of disks.
        * 4x100GB ports (!?).
    * 1U _"grey"_ servers (Flash servers / Overload usage (think new hit
      series)).
        * 2x 40GB NICS / 6x 100GB NICS (!?).
    * Use a mixture of both.
    * Sync with bittorrent (like).
    * Mixture of Netflix owned sites and ISP housed kit (given for free) in
      local POPs. eg. Cabinets in Cardiff BT racks, 2x installs in Fishguard.
    * Decisions on whether to push content more locally or keep it further out
      (if niche).
    * Move stuff nightly, both between POP's or disk to disk within a server.
    * All devs have a _"RED"_ (!?) box under their desk that is serving 10TB of
      content a day!!
* Metrics:
    * Cassandra on disks is too slow for live analysis.
    * Store metrics in memory for 2 weeks, on Petabytes of RAM, then dump to
      Elastic Search (Fast but expensive).
* AWS Issues:
    * Pushing AWS for 100GB links (currently 10GB) in USA.
    * 600GB (!?) trans-atlantic (x3 for redundancy) and 100GB in EU.
    * AWS can be slow.
        * Trying Control Plane from local Netflix POPs instead of USA
          (codenamed FTL).
        * Compute at Edge.
* Open Source:
    * Nginx creator now works in Netflix.
    * Everyone commits to mainline of open source projects (ie. no sitting on
      patches internally).
        * Netflix Open Connect.

Eliminating Snowflake Dev Environments (Ed)
-------------------------------------------

Interesting how they containerise the entire dev platform as opposed to
containerising the build environment only.

* [12 Factor] - _"environment parity"_.
* [Github: FINkit/development-environment-base].
* [VirtualBox] images for devs.
    * [Hashicorp: Packer].
    * [Ansible] scripts.
    * [Hashicorp: Vagrant] (dev box deployment).
    * [VirtualBox] (OS, dependencies, IDE's, Lastpass, configuration, repos,
      custom CLI).
* Gives a unified dev environment for all.

Reproduceability & Productivity in Data Science & AI (Mark Coleman)
-------------------------------------------------------------------

Very good talk using the Dev history of the slow process driven deployments
during Waterfall and no Version Control era, to the hyper fast deployments of
the current version controlled enabled CI/CD age.

* [DotScience], mark@dotscience.com.
* Debugging is the key to software engineering !!
* [The Joel Test: 12 steps to better code] - published 2005, by Joel of
  StackOverflow.
* Destructive Collaboration - slow.
    * No version control.
    * Forked, uncontrolled code/artifacts.
* Constructive Collaboration - faster working.
    * Version Control is ubiquitous.
    * CI - removes _"test-only" teams, due to automated testing done in dev
      cycle.
    * CD - removes _"Operations-only" teams, due to expressing deployments as
      an API.
    * Pull everyone into the development cycle.
    * Good testing & processes = Speed + Safety.
        * It's quick to deploy, so fuck it, deploy, let it break and push
          fixes.
* Data Science is very backwards (90's) in processes.
    * Minimal testing, no VC, manual deployment...
* VC is the key for Data Science.
    * Git can have issues (git-lfs may help??) due to big data (TB's of data).
        * Pimping their product: _Dotmesh_ on ZFS.
* Testing on big datasets is hard.
    * Cloud style testing: hit large datasets & monitoring that it doesn't
      deviate to far from the expected case.


[Meetup: Cloud Native Wales event]: https://www.meetup.com/Cloud-Native-Wales/events/250631346/

[Netflix Culture]: 2018-11-08_cloud_native_wales
[Netflix: Simian Army]: https://medium.com/netflix-techblog/the-netflix-simian-army-16e57fbab116
[Chaos Monkey]: https://github.com/Netflix/chaosmonkey

[12 Factor]: https://12factor.net
[Github: FINkit/development-environment-base]: https://github.com/finkit/development-environment-base
[VirtualBox]: https://www.virtualbox.org
[Hashicorp: Packer]: https://www.packer.io
[Ansible]: https://docs.ansible.com/ansible/latest/index.html
[Hashicorp: Vagrant]: https://www.vagrantup.com

[DotScience]: https://dotscience.com
[The Joel Test: 12 steps to better code]: https://www.joelonsoftware.com/2000/08/09/the-joel-test-12-steps-to-better-code/
