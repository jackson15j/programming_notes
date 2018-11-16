VPN Notes
=========

VPN Security/Usability (Best to Worst):

* IKEv2 (UDP: 500)(Resilient to interface changes(auto-reconnect)(MOBIKE))(Same
  as L2TP/IPSec, but fewer overheads (faster), some platforms may not support
  it).
* OpenVPN (TCP: 443)(OpenSSL based
  SSLv3TLSv1/3DES(compromised)/AES/RC5/Blowfish encryption, Software VPN =
  maintenance cost to harden/update OS vulnerabilities).
* L2TP/IPSec (Encapsulates PPP)(UDP: 500)(AES-128/192/256 / 3DES (compromised)
  encryption, data integrity, origin authentication, double encapsulation =
  time/cpu hit (negligible these days).
* SSTP (Encapsulates PPP)(TCP: 443)(Microsoft owned, SSLv3, data integrity,
  origin authentication).
* PPTP (Encapsulates PPP)(128bit encryption (compromised), no data integrity
  checking, no origin authentication, not HIPPA/PCI compliant).

Mostly a summary of: [SpeakNetworks: PPTP vs L2TP/IPSec vs SSTP vs IKEv2 vs
OpenVPN].

IPSec Policy vs Route based VPNs
--------------------------------

Policy-based VPN:

* Uses configuration to define both sides network structure (subnets).
* Policy determines if traffic directed at the tunnel is encrypted or not.
* Tunnel is spun up on presence of traffic (Seems to be always up in my IKE
  testing so far).
* Policy creates a new SA pairing per tunnel.

Route-based VPN:

* Virtual interfaces for VPN Traffic.
* All traffic directed at the tunnel is encrypted.
* Use OS routing (`iptables`, `ip route`) to define what traffic is directed to
  the tunnel.
* Only need to know destination VPN (not subnet(s) behind destination VPN) in
  routing config.
* Simpler to manage.
* Easier to scale to multiple VPN connections.

Glossary
--------

* Remote Access: Hosts require a VPN Client software/hardware to tunnel into a
  site.
* L2L: Lan to Lan - _"Always On"_ VPN tunnel between two sites. Hosts do not
  require a VPN Client (ie. seamless from their point of view).
* S2S: Site to Site - See `L2L`.
* PPP: Point to Point Protocol - encapsulates IP packets within PPP frames and
  then transmits the encapsulated packets across the network/internet.
* PPTP: Point to Point Tunneling Protocol - Encrypts and tunnels PPP packets.
* L2F: Layer 2 Forwarding - Cisco designed protocol.
* IPSec: IP Security. A suite of protocols for cryptographically securing
  communications at the IP Packet Layer.
* L2TP/IPSec: Combination of PPTP and L2F. Tunneling based off PPP
  specification. Encryption via IPSec.
* SSTP: Secure Socket Tunneling Protocol - encapsulates PPP traffic over SSL
  (Secure Sockets Layer).
* IKE: Internet Key Exchange - Uses IPSec in tunnel mode. Supports mobility
  (MOBIKE) to be tolerant to network/interface changes.

Link Dump
---------

* [StrongSwan].
    * [StrongSwan: Usable Example Configs].
    * [StrongSwan: AWS VPC].
    * [AWS VPN Solutions with StrongSwan].
    * [S2S VPN between OpenVPN (via StrongSwan) & AWS].
    * [Medium: S2S IPSec VPN on GCP/AWS with StrongSwan].
* [Github: patrickbcullen/Openswan-VPC].
* [LibreSwan].
    * [Ggithub: libreswan/libreswan].
* [OpenVPN].
* [Xmodulo: Connect LAN to Amazon Virtual Private Cloud].
* [AWS: VPN Connections].
* [SpeakNetworks: PPTP vs L2TP/IPSec vs SSTP vs IKEv2 vs OpenVPN].
* [Docker: hwdsl2/ipsec-vpn-server].
* [Juniper: IPSec VPN Overview].
* [PacketLife: Policy vs Route-based VPNs (Part 1)], [PacketLife: Policy vs
  Route-based VPNs (Part 2)].
* [SysTutorials: Linux Gateway with iptables & route].
* [SysTutorials: Port forwarding using iptables].


[StrongSwan]: https://strongswan.org
[StrongSwan: Usable Example Configs]: https://wiki.strongswan.org/projects/strongswan/wiki/UsableExamples
[StrongSwan: AWS VPC]: https://wiki.strongswan.org/projects/strongswan/wiki/AwsVpc
[AWS VPN Solutions with StrongSwan]: https://www.maxmanders.co.uk/2014/05/05/aws-vpn-solutions-with-strongswan.html
[Medium: S2S IPSec VPN on GCP/AWS with StrongSwan]: https://medium.com/the-andela-way/site-to-site-ipsec-vpn-on-gcp-aws-with-strongswan-cb46dff1ab37
[S2S VPN between OpenVPN (via StrongSwan) & AWS]: https://aravindkrishnaswamy.wordpress.com/2014/11/26/site-to-site-vpn-between-openvpn-and-aws/
[Github: patrickbcullen/Openswan-VPC]: https://github.com/patrickbcullen/Openswan-VPC
[LibreSwan]: https://libreswan.org
[Ggithub: libreswan/libreswan]: https://github.com/libreswan/libreswan
[OpenVPN]: https://openvpn.net
[Xmodulo: Connect LAN to Amazon Virtual Private Cloud]: http://xmodulo.com/connect-lan-amazon-virtual-private-cloud.html
[AWS: VPN Connections]: https://docs.aws.amazon.com/vpc/latest/userguide/vpn-connections.html
[SpeakNetworks: PPTP vs L2TP/IPSec vs SSTP vs IKEv2 vs OpenVPN]: https://www.speaknetworks.com/pptp-vs-l2tpipsec-vs-sstp-vs-ikev2-vs-openvpn/
[Docker: hwdsl2/ipsec-vpn-server]: https://hub.docker.com/r/hwdsl2/ipsec-vpn-server/
[Juniper: IPSec VPN Overview]: https://www.juniper.net/documentation/en_US/junos/topics/topic-map/security-ipsec-vpn-overview.html
[PacketLife: Policy vs Route-based VPNs (Part 1)]: http://packetlife.net/blog/2011/aug/15/policy-based-vs-route-based-vpns-part-1/
[PacketLife: Policy vs Route-based VPNs (Part 2)]: http://packetlife.net/blog/2011/aug/17/policy-based-vs-route-based-vpns-part-2/
[SysTutorials: Linux Gateway with iptables & route]: https://www.systutorials.com/1372/setting-up-gateway-using-iptables-and-route-on-linux/
[SysTutorials: Port forwarding using iptables]: https://www.systutorials.com/816/port-forwarding-using-iptables/

