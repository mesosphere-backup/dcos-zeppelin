# zeppelin-docker

This is the docker build for Zeppelin on DC/OS.

## Documentation

All [DC/OS Zeppelin documentation](docs/table-of-contents.md) is in the `docs` folder of this repository.

## Building

```sh
cd docker
docker build -t mesosphere/zeppelin:<version> .
```

## Installing

These are instructions for installing and accessing the DC/OS Zeppelin
package.  We'll probably move these to user docs at some point.

The following are instructions for installing Zeppelin on an AWS DC/OS
cluster such that you can access it from the public internet using
marathon-lb as a proxy.  An alternative is to launch Zeppelin directly
on a a public slave.

1. Install marathon-lb

   ```sh
   dcos package install marathon-lb
   ```

2. Install zeppelin

   ```sh
   dcos package install zeppelin --options=zeppelin.json
   ```

   zeppelin.json:
   ```json
   {
       "zeppelin": {
           "vhost": "zeppelin.your-url.com"
       }
   }
   ```

3. Configure ELB

   You must modify your ELB in two ways.

   The first is to update the health check so that marathon-lb will show
   up as a backend to the ELB.  Add "HTTP:9090/_haproxy_health_check".

   The second is to change the `HTTP:80` listener to be `TCP:80`.  This is to
   support Zeppelin websockets.

4.  Create a DNS entry for zeppelin.your-url.com

    Create a CNAME entry that maps the `vhost` parameter above to the ELB
    DNS entry.  In development, you can simply add an entry to your
    `/etc/hosts` file.

You should now be able to visit zeppelin.your-url.com to view Zeppelin.
