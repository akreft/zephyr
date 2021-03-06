.. _ip_stack_overview:

Overview
########

Supported Features
******************

The networking IP stack is modular and highly configurable via build-time
configuration options. You can minimize system memory consumption by enabling
only those network features required by your application. Almost all features
can be disabled if not needed.

* **IPv6** The support for IPv6 is enabled by default. Various IPv6 sub-options
  can be enabled or disabled depending on networking needs.

  * Developer can set the number of unicast and multicast IPv6 addresses that
    are active at the same time.
  * The IPv6 address for the device can be set either statically or
    dynamically using SLAAC (Stateless Address Auto Configuration) (RFC 4862).
  * The system also supports multiple IPv6 prefixes and the maximum
    IPv6 prefix count can be configured at build time.
  * The IPv6 neighbor cache can be disabled if not needed, and its size can be
    configured at build time.
  * The IPv6 neighbor discovery support (RFC 4861) is enabled by default.
  * Multicast Listener Discovery v2 support (RFC 3810) is enabled by default.
  * IPv6 header compression (6lo) is available for IPv6 connectivity for
    Bluetooth IPSP (RFC 7668) and IEEE 802.15.4 networks (RFC 4944).

* **IPv4** The legacy IPv4 is supported by the networking stack. It cannot be
  used by IEEE 802.15.4 or Bluetooth IPSP as those network technologies support
  only IPv6. IPv4 can be used in Ethernet based networks. By default IPv4
  support is disabled.

  * DHCP (Dynamic Host Configuration Protocol) client is supported (RFC 2131).
  * The IPv4 address can also be configured manually. Static IPv4 addresses
    are supported by default.

* **Dual stack support.** The networking stack allows a developer to configure
  the system to use both IPv6 and IPv4 at the same time.

* **UDP** User Datagram Protocol (RFC 768) is supported. The developer can
  send UDP datagrams (client side support) or create a listener to receive
  UDP packets destined to certain port (server side support).

* **TCP** Transmission Control Protocol (RFC 793) is supported. Both server
  and client roles can be used the the application. The amount of TCP sockets
  that are available to applications can be configured at build time.

* **BSD Sockets API** Support for a subset of a BSD Sockets compatible API is
  implemented. Both blocking and non-blocking datagram (UDP) and stream (TCP)
  sockets are supported.

* **Secure Sockets API** Experimental support for TLS/DTLS secure protocols and
  configuration options for sockets API. Secure functions for the implementation
  are provided by mbedTLS library.

* **MQTT** Message Queue Telemetry Transport (ISO/IEC PRF 20922) is supported.
  A sample :ref:`mqtt-publisher-sample` client application for MQTT v3.1.1 is
  implemented.

* **CoAP** Constrained Application Protocol (RFC 7252) is supported.
  Both :ref:`coap-client-sample` and :ref:`coap-server-sample` sample
  applications are implemented.

* **LWM2M** OMA Lightweight Machine-to-Machine Protocol (V1.0 Feb 2017) is
  supported via the "Register Device" API (Register, De-Register and Update)
  and has template implementations for Security, Server, Device Management and
  Firmware objects. Bootstrap support is currently not supported.
  :ref:`lwm2m-client-sample` implements the library as an example.

* **DNS** Domain Name Service (RFC 1035) client functionality is supported.
  Applications can use the DNS API to query domain name information or IP
  addresses from the DNS server. Both IPv4 (A) and IPv6 (AAAA) records can
  be queried.

* **Network Management API.** Applications can use network management API to
  listen management events generated by core stack when for example IP address
  is added to the device, or network interface is coming up etc.

* **Multiple Network Technologies.** The Zephyr OS can be configured to
  support multiple network technologies at the same time simply by enabling
  them in Kconfig: for example, Ethernet and 802.15.4 support. Note that no
  automatic IP routing functionality is provided between these technologies.
  Applications can send data according to their needs to desired network
  interface.

* **Minimal Copy Network Buffer Management.** It is possible to have minimal
  copy network data path. This means that the system tries to avoid copying
  application data when it is sent to the network.

* **Virtual LAN support.** Virtual LANs (VLANs) allow partitioning of physical
  ethernet networks into logical networks. See :ref:`vlan_interface` for more
  details.

* **Network traffic classification.** The sent and received network packets can
  be prioritized depending on application needs.
  See :ref:`traffic-class-support` for more details.

* **Time Sensitive Networking.** The gPTP (generalized Precision Time Protocol)
  is supported. See :ref:`gptp_interface` for more details.

* **Network shell.** The network shell provides helpers for figuring out
  network status, enabling/disabling features, and issuing commands like ping
  or DNS resolving. The net-shell is useful when developing network software.
  See :ref:`net_shell` for more details.

Additionally these network technologies (link layers) are supported in
Zephyr OS v1.7 and later:

* IEEE 802.15.4
* Bluetooth
* Ethernet
* SLIP (IP over serial line). Used for testing with QEMU. It provides
  ethernet interface to host system (like Linux) and test applications
  can be run in Linux host and send network data to Zephyr OS device.

Source Tree Layout
******************

The networking stack source code tree is organized as follows:

``subsys/net/ip/``
  This is where the IP stack code is located.

``subsys/net/l2/``
  This is where the IP stack layer 2 code is located. This includes generic
  support for Bluetooth IPSP adaptation, Ethernet, IEEE 802.15.4 and WiFI.

``subsys/net/lib/``
  Application-level protocols (DNS, MQTT, etc.) and additional stack
  components (BSD Sockets, etc.).

``include/net/``
  Public API header files. These are the header files applications need
  to include to use IP networking functionality.

``samples/net/``
  Sample networking code. This is a good reference to get started with
  network application development.

``tests/net/``
  Test applications. These applications are used to verify the
  functionality of the IP stack, but are not the best
  source for sample code (see ``samples/net`` instead).
