# Instructions

To set up the registry, we need to create a self-signed certificate. 

## Creating a self-signed certificate

*Replace `$domainname` with the domain name you want your docker registry to be reachable under.*

See the [documentation](https://docs.docker.com/registry/insecure/#use-self-signed-certificates) for reference.

Run these commands:

```
$ domainname=registry.local
$ openssl req \
  -newkey rsa:4096 -nodes -sha256 -keyout $domainname.key \
  -x509 -days 365 -out $domainname.crt
```

Then you can enter some information that will apear in your certificate, but you can skip most of it by entering `.` - be sure to enter `$domainname` as the CN though.

Now copy `$domainname.crt` to `/etc/docker/certs.d/$domainname/ca.crt` on every Docker Host.

Like this:

```bash
sudo mkdir -p /etc/docker/certs.d/registry.local/
sudo cp registry.local.crt /etc/docker/certs.d/registry.local/ca.crt
```

# Pushing to this registry

We now configured the registry with a certificate it's waiting for us on Port 443 inside the container - there's just one problem: Our Dockerhost is doing the push and it doesn't know about the DNS-Names for containers (well on some level it obviously knows, but it doesn't publish them). To change this, we need to find out the IP of our registry-container and add an entry to `/etc/hosts` for it.

To get the IP, run `host $domainname` from **inside** one of the containers on the same network as the registry - or the registry as well.

Then edit `/etc/hosts` and add a line like this:
```
$container-ip    $domainname
```

Voilà - when you `curl https://$domainname/v2/ --insecure` you shold get an empty JSON response: `{}`  