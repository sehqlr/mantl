Marathon
========

!!! note ""
	new in version 0.1.0

[Marathon](http://mesosphere.github.io/marathon/) is a scheduler for
mesos - it takes specifications for apps to run and lets you scale them
up and down, and deploy new versions or roll back. Like Mesos' leader
mode, Marathon can run on as many servers as you like and will elect a
leader among nodes using zookeeper.

Keep Marathon servers close to Mesos leaders for best performance; they
talk back and forth quite a lot to keep the services in the cluster in a
good state. Placing them on the same machines would work.

Marathon listens on port 8080. To connect to Marathon securely, set
marathon\_keystore\_path and marathon\_keystore\_password, then connect
via HTTPS on port 8443.

The Marathon role also sets up
[mesos-consul](https://github.com/CiscoCloud/mesos-consul) and
[marathon-consul](https://github.com/CiscoCloud/marathon-consul) for
service discovery.

Variables
---------

marathon\_http\_credentials
:   HTTP Basic authentication credentials, in the form "user:password".
:   **format**: `"user:password"`

marathon\_keystore\_path
:	Path on the local machine that contains a Java keystore. Marathon has docs on [generating this file](https://mesosphere.github.io/marathon/docs/ssl-basic-access-authentication.html).
:   Please note that if this option is set, marathon\_keystore\_password is *required*.

marathon\_keystore\_password
:	Password for the keystore specified in marathon\_keystore\_path.

marathon\_principal
:	Principal to use for Mesos framework authentication.
:	!!! note
		If you plan to use framework authentication, be sure to add the principal and secret to mesos\_credentials and set mesos\_authenticate\_frameworks to `yes`.
:	**default**: `marathon`

marathon\_secret
:	Secret to use for Mesos framework authentication. Authentication will only be enabled if this value is set to a non-blank value. See also the note in marathon\_principal.
:	**default**: `""`

mesos\_consul\_image
:	Image for the [mesos-consul](https://github.com/CiscoCloud/mesos-consul) bridge.
:	**default**: `drifting/mesos-consul`

mesos\_consul\_image\_tag
:	Tag for the [mesos-consul](https://github.com/CiscoCloud/mesos-consul) bridge
:	**default**: `latest`

marathon\_consul\_image
:	Image for the [marathon-consul](https://github.com/CiscoCloud/marathon-consul) bridge.
:	**default**: `brianhicks/marathon-consul`

marathon\_consul\_image\_tag
:	Tag for the [marathon-consul](https://github.com/CiscoCloud/marathon-consul) bridge
:	**default**: `latest`

marathon\_logging\_level
:	Log level for Marathon
:	**default**: `warn`
