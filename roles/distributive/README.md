Distributive
============

Added in version 1.1

[Distributive](https://www.consul.io/) is used in Mantl to run detailed,
granular health checks for various services.

This role is run several times as a dependency for other roles.

Variables
---------

You can use these variables to customize your Distributive installation.

### distributive\_interval

The interval between running distributive checks. Default is "1m"

### distributive\_timeout

The timeout for running distributive checks. Default is "30s".

### checklist\_versions

The version of the checklist package to install for the specified role.
Defaults are different for each role, but are of the form e.g.
`consul: 0.2.4-1`.
