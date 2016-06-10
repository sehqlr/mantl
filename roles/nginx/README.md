Nginx
=====

[Nginx](http://nginx.org/) is a web and proxy server. Mantl uses it in
front of the mesos, marathon, and consul web UIs to provide basic
authentication and SSL. Those proxies are set up in the individual roles
linked above, and the base `nginx` role is just used to move the
relevant certificates into place.
