mantlui =====

Version Added:: 0.4

Mantlui consolidates the web UIs of various components in Mantl,
including Mesos, Marathon, Chronos, and Consul at a single url on port
80 (http) or 443 (https).

-   

    \[x\] Mesos UI (/mesos)

    :   -   \[x\] Fixes mesos leader redirection (don't have to go to
            the ui on the leader now)
        -   \[x\] Stream mesos logs
        -   \[x\] View mesos task sandbox
        -   \[x\] Stream mesos task stderr/stdout (and other artifacts)
        -   \[x\] View/download mesos task artifacts

-   \[x\] Marathon UI (/marathon)
-   \[x\] Chronos UI (/chronos)
-   \[x\] Consul UI (/consul)

Variables
=========

You can use these variables to customize your Mantlui installation.

> mantlui\_nginx\_image
>
> > nginx-mantlui docker container image name
> >
> > default: `ciscocloud/nginx-mantlui`
>
> mantlui\_nginx\_image\_tag
>
> > nginx-mantlui docker container image tag
> >
> > default: `0.6.6`
>
> do\_mantlui\_ssl
>
> > Use https to secure the mantlui.
> >
> > default: `false`
>
> do\_mantlui\_auth
>
> > Use basic authentication to secure the mantlui.
> >
> > default: `false`
