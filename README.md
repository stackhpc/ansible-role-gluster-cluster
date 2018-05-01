Gluster Cluster
===============

This is a lightweight and opinionated role for configuring a Gluster cluster,
serving a single volume and spanning multiple nodes.

Requirements
------------

* Only Centos hosts are currently supported.

* The hosts must be homogeneous with respect to block devices. In particular
  the block devices specified in  ```gluster_cluster_block_devices``` should
  exist in all nodes within the cluster.

Role Variables (required)
-------------------------

* ``gluster_cluster_volume_name``: Name of the Gluster volume to create.
* ``gluster_cluster_block_devices``: List of block devices to create bricks from.
* ``gluster_cluster_transport_interface``: For example: ``ib0``, ``enp0s1``.
* ``gluster_cluster_transport_mode``: Either ``tcp`` or ``rdma``.


Role Variables (optional)
-------------------------

* ``gluster_cluster_hosts``: Defaults to all hosts in the
  ``gluster_cluster_storage_group_name`` group. The hosts which provide the bricks
  for the volume provided by the Gluster cluster.
* ``gluster_cluster_storage_group_name``: Defaults to ``storage``. The Ansible
  group name of storage hosts from the inventory file.
* ``gluster_cluster_volume_options``: A dict of Gluster volume options.
* ``gluster_cluster_volume_base_path``: The base folder in which to store volumes.
* ``gluster_cluster_stripes``: Default to ``0``. See [1].
* ``gluster_cluster_disperses``: Defaults to ``0``. See [1].
* ``gluster_cluster_replicas``: Defaults to ``0``. See [1].
* ``gluster_cluster_redundancies``: Defaults to ``0``. See [1].

[1] https://docs.gluster.org/en/v3/Administrator%20Guide/Setting%20Up%20Volumes/

Dependencies
------------

None

Example Playbook
----------------

The following playbook configures a Gluster volume:

    ---
    - name: Configure Gluster
      hosts: storage
      roles:
        - role: gluster-cluster
          gluster_cluster_volume_name: my_volume
          gluster_cluster_block_devices:
            - sdb
            - sdc
            - sdd
          gluster_cluster_transport_interface: ib0
          gluster_cluster_transport_mode: rdma
          gluster_cluster_volume_options:
            cluster.nufa: 'on'

Author Information
------------------

- Doug Szumski (<doug@stackhpc.com>)
