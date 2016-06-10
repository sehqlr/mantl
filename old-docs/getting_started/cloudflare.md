CloudFlare
==========

Version Added:: 0.5

Terraform can use CloudFlare to provide DNS records for your cluster,
independent of which provider you use to provision your servers.

CloudFlare Username/Token
-------------------------

The easiest way to configure credentials for CloudFlare is by setting
environment variables:

ENVVAR:: CLOUDFLARE\_EMAIL

> Your e-mail address for the CloudFlare account

ENVVAR:: CLOUDFLARE\_TOKEN

> The CloudFlare token (found in the CloudFlare admin panel)

Alternatively, you can set up the CloudFlare provider credentials in
your .tf file:

``` {.sourceCode .shell}
provider "cloudflare" {
  email = "the e-mail address for your CloudFlare account"
  token = "your CloudFlare token"
}
```
