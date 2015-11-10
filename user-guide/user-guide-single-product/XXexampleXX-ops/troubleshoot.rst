.. _troubleshoot:

---------------------------
Troubleshooting XXexampleXX
---------------------------
Many of the same troubleshooting skills you already know apply
to XXexampleXX as well.

When you run into a problem, try to eliminate as many unknowns
as possible. Try to get the simplest possible example of what you
want to achieve working first.

However, there are specific areas
that may be new to you. Here are some tips which may help you
if you run into issues:

Troubleshooting connectivity
~~~~~~~~~~~~~~~~~~~~~~~~~~~~
When dealing with connectivity issues, regardless of whether they are
client-to-server or server-to-server, it might be useful to go
through the checklist below:

* If a connection to a server fails,
  try reaching the same server from different network locations,
  devices, and directions.
  If any method succeeds, the problem is not in the server itself
  but in some aspect of the network.

* Try to access the server by
  its IP address
  (for example, 192.0.2.0)
  instead of its
  DNS entry (for example, www.example.com).
  If this succeeds, the problem relates to the DNS rather than
  to the server itself.

* Try to confirm basic TCP/IP connectivity by using
  `telnet <https://tools.ietf.org/html/rfc854>`__.
  If you cannot telnet to a target IP through a target port,
  a firewall may be blocking your access.

* For detailed steps on troubleshooting networking or DNS issues, see
  these guides:

  * :kc-article:`Basic Network Troubleshooting <basic-network-troubleshooting>`

  * :kc-article:`Troubleshooting DNS Issues <troubleshooting-dns-issues>`

Troubleshooting servers
~~~~~~~~~~~~~~~~~~~~~~~
* Keep in mind that Windows servers normally take
  longer to build than other operating systems.

* If a build fails, try again. The build system is often
  busy and doesn't always succeed; try more than once before being
  sure of a problem.

Other troubleshooting resources
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Rackspace provides a
`real-time system status view <https://status.rackspace.com>`__
that may help you understand whether a problem is unique to you or
is caused by something more general.

You can also subscribe to our
`@Rackstatus <https://twitter.com/rackstatus>`__
Twitter feed for updates on maintenance or
other service-impacting events.

As a managed cloud company, we're always available for you to
contact us directly. If you need help, you can reach out to us
through the "Support"
link at the top of the
:mycloud:`Rackspace Cloud Control Panel <>`.
