---
post_title: Install
menu_order: 0
enterprise: 'no'
---

Use the following command from the DC/OS CLI to install Zeppelin:
 
```
$ dcos package install zeppelin
```

**Note:** DC/OS Zeppelin cannot be installed in the `strict` [security mode](https://docs.mesosphere.com/1.8/administration/installing/custom/configuration-parameters/#security) of Enterprise DC/OS.

# Install on DC/OS 1.8 and above
On DC/OS >= 1.8 access Zeppelin at
`http://<dcos_url>/service/zeppelin/` after running the above command.

**Note:** Zeppelin uses websockets, which communicate over raw TCP
sockets, rather than HTTP.  On AWS DC/OS 1.8, the ELB to which
`<dcos_url>` resolves will proxy HTTP traffic, but not TCP.  For
Zeppelin to work properly, you must downgrade the ELB port 80 proxy to
TCP.

On DC/OS <1.8, you can access Zeppelin from the public internet by using marathon-lb [as described here](https://docs.mesosphere.com/1.8/usage/service-discovery/marathon-lb/usage/).

<!-- 
Alternately, you can deploy Zeppelin on a public agent by setting the
`acceptedResourceRoles` field of the Marathon app to
`["slave_public"]`.  You can then access Zeppelin By combining the
public IP address of the agent running Zeppelin with the `$PORT0` of
its marathon app.
-->
