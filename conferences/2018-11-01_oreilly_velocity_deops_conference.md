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


[O'Reilly: Velocity (EU)]: https://conferences.oreilly.com/velocity/vl-eu
[O'Reilly: Velocity 2018-11-01 Schedule]: https://conferences.oreilly.com/velocity/vl-eu/schedule/2018-11-01
[O'Reilly: Velocity 2018-11-02 Schedule]: https://conferences.oreilly.com/velocity/vl-eu/schedule/2018-11-02


[Amazon: Docker on Windows (Elton Stoneman)]: https://www.amazon.co.uk/Docker-Windows-101-Production-ebook/dp/B0711Y4J9K
[SafariBooks: Distributed Systems Observability (Cindy Sridharan)]: https://www.oreilly.com/library/view/distributed-systems-observability/9781492033431/


[Things you can't cloud your way out of (Jessica Brown, Fastly)]: https://conferences.oreilly.com/velocity/vl-eu/public/schedule/detail/72252
[Fastly: Balancing Requets]: https://www.fastly.com/blog/building-and-scaling-fastly-network-part-2-balancing-requests
