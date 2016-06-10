Haproxy
=======

Version Added:: 0.2

> **warning**
>
> Haproxy is deprecated as of Mantl 0.5. Please migrate to traefik at
> your earliest convenience.

[Haproxy](https://github.com/CiscoCloud/haproxy-consul) connects to
consul, and proxies all registered services. At the current time all
containers are mapped to port 80. Future versions will allow each
container to control the haproxy port and url mapping.

Variables
---------

Depends on `haproxy_domain`, as we use host\_acl on the hostname in the
request.

> haproxy\_image
>
> > default: `asteris/haproxy-consul`
>
> haproxy\_image\_tag
>
> > default: `latest`
>
> haproxy\_domain
>
> > default: `example.com`
> >
> > The domain that haproxy will use to match url requests. For example
> > if this is set to example.com, apps `myapp` and `hello-world` will
> > be mapped to `myapp.example.com`, `hello-world.example.com`.
> >
> > For this to work, you need to have a wildcard dns entry for
> > `*.example.com`, so when your browser goes to
> > `http://myapp.example.com`, your DNS settings forward that traffic
> > to one of the nodes running haproxy.

Making haproxy work with the DNS setup from Terraform
-----------------------------------------------------

The Terraform files in Mantl are set up to create DNS records in the
form of `*.[short-name]-lb.[domain]`, e.g. \*.mi-lb.example.com. In
order to set up haproxy to work with these DNS records, set
haproxy\_domain to `[short-name]-lb.[domain]`, replacing `[short-name]`
and `[domain]` with your values. Then, you can access `myapp` at
`myapp.[short-name]-lb.[domain]` (`myapp.mi-lb.example.com`)
