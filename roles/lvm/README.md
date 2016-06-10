LVM
===

Version Added:: 0.5

The lvm role optionally creating LVM Volume Group, specifically:

-   Install required software and tools.
-   Enable lvmetad daemon.
-   Create volume group and add provided extra block device to it as
    physical volume.
-   Register fact with name of created volume group.

Variables
---------

> lvm\_volume\_group\_name
>
> > Volume group name to be created.
> >
> > default: `mantl`
>
> lvm\_physical\_device
>
> > Device name for attach as volume group.
> >
> > Default value can vary depending from used cloud.
> >
> > -   For Openstack: /dev/vdb
> > -   For GCE: /dev/disk/by-id/google-lvm
> > -   For AWS: /dev/xvdh

Facts
-----

> volume\_group\_name
>
> > This fact set after volume creation, and used later by docker and
> > glusterfs roles.
> >
> > default: `None` (if no LVM used)
