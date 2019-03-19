2019-03-04 Cloud Native Wales
=============================

Intro
-----

- https://cloudnativewales.io
    - https://cloudnativewales.io/post/014_marchmeetup/
    - Livestream: https://youtu.be/5c8gB1U3YPA
- Lewis talking next may at Kubecon in Barcelona.
- https://leank8s.io may29th Cardiff
- was Meetup 18th. Serverless.
- kubernetes on gcloud 28th March.
- ai Wales 4th April
- skills matter (conferences in London)


Brendan Collins - algorithmia serverless micro services platform for ai
-----------------------------------------------------------------------

(Okay. High level of cloud abstraction. Minimal about the meat of the tech).

- office of national statistics.
- enterprise solution architect.
- algorithmia - serverless platform.

History

- App Store for ai platforms
- open web
- place to share data models for data scientists to do machine learning.
- share for free or money.


- algorithmia.com
    - Invite code: CardiffMeetup ($30 credit).
- run models direct from the site.
- can use python/R models in java/etc infrastructure.
- allows for models to be dropped into a scaleable platform.
- no DevOps learning required for the data scientists.
    -

Serverless:

- cost, improved latency (geolocated), concurrency built in, efficient, speed
  of development.
- they run GPU backed servers for Tensaflow calculations !?

Microservices:

- docker on kubernetes with REST apis.
- SW/HW agnostic.
-

OS for ai:

- abstraction:
    - overarching shell & services.
    - elastic scale - Web/api load balancer's. Worker nodes management
      (geolocated nodes). Model costs (cpu/mem).
    - runtime abstraction - models in different languages, _potentially_
      chained to models of other languages.
    - cloud abstraction - different providers, different container types,
      geolocated nodes, etc made easy to connect to concisely.
- comparability.
    - ai pipelines are typically chained models.
    - elastic scale kernel handles this.



Stig Telfer - openstack and the software defined super computer
---------------------------------------------------------------

- stackHPC ltd
    - https://www.stackhpc.com
    - CTO. Small start up. Bristol.
- LIGO !?
    - 2x 4km vacuum tubes at right angles with a mirror and laser to measure
      gravity wave events.
    - measure big events; black holes crashing together.
    - not very sensitive.
- future: square kilometre array
    - sensitive telescopes to measure. Network of pulsars that emit regular
      waves, to measure galaxy wide events. Plus get paste local noise.
    - 2020.
    - ~700GB/s of data. Along 7x100GB links.
    - ~40 Petabytes of hot cache (1hr of data).
- they are making a system to work in this chain. Called: ALaSKA.
    - help data scientists for the 2024 goal.
    - fast/bulky racks of kit running openstack bare metal (no virtualisation
      layer!!). Mini super computer.

Their stack:

- openstack that runs up other platforms as required.
- openstack with bare metal clusters/platforms.
    - configurations to get best out of the physical kit.
- ansible.
- beeGFS - fast file system
- slurm - stats !?
    - display all bottlenecks (instead of abstraction like other cloud
      providers to hide slow gear).
- Euclid, IRIS and openstack ( Cambridge and RAL).
    - 6000 cores, 3 months.  IRIS: federation of uk scientists of compute
    resources (Edinburgh, Cambridge, RAL, ...).
- challenge: local SDN super computer clusters in sites that can also be linked
  via VPN meshes across federated links. With local and remote data dumps.
    - optimised cloud to match bare metal speeds ( bios tweaks, passing
      physical network cards, cpu socket awareness, bare metal hypervisors,
      ...)
    - lots of vm flexibility removed to get speed (eg. Migration).
- Cambridge has biggest super computer (but still need 60 to solve the Future
  problem).
    - their _”data accelerator”_ is now in to optimise the jobs.
    - mini transparent caches created for jobs to not affect others. - cloud
      native mentality.
    - got applications running closer to the theoretical limits of the disks
      and network (517GB/s !?).
    - Chasing CERN for data speeds. They only use openstack.

DNA gene sequencing:

- cost drastically dropped (faster than murphies law).
- the 100k genome project (uk).
    - opencb.
- tech stack is private cloud stack.

Use case:
- 1 user
- 1 image, deployed to fill the cloud.
- opposite to cloud mentality of lots of users and lots of instances being
  pushed at random times.

- inmos - processor company.
