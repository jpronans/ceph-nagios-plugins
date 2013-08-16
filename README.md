# Nagios plugins for Ceph

A collection of nagios plugins to monitor a [Ceph][] cluster.

## ceph-health

The `ceph-health` nagios plugin montiors the ceph cluster, and report its health.

### Usage

    usage: ceph-health [-h] [-e EXE] [-c CONF] [-m MONADDRESS] [-i ID] [-k KEYRING] [-d]

    'ceph health' nagios plugin.

    optional arguments:
      -h, --help            show this help message and exit
      -e EXE, --exe EXE     ceph executable [/usr/bin/ceph]
      -c CONF, --conf CONF  alternative ceph conf file
      -m MONADDRESS, --monaddress MONADDRESS
                            ceph monitor address[:port]
      -i ID, --id ID        ceph client id
      -k KEYRING, --keyring KEYRING
                            ceph client keyring file
      -d, --detail          exec 'ceph health detail'


### Authentication

Ceph is normally configured to use [cephx] to authenticate its client. 

To run the `ceph-health` plugin as user `nagios` you have to create a special keyring:

    root# ceph auth get-or-create client.nagios mon 'allow r' > client.nagios.keyring

And use this keyring with the plugin:

    nagios$ ceph-health --id nagios --keyring client.nagios.keyring
    
### Example

    nagios$ ./ceph-health --id nagios --keyring client.nagios.keyring
    HEALTH WARNING: 1 pgs degraded; 1 pgs recovering; 1 pgs stuck unclean; recovery 4448/28924462 degraded (0.015%); 2/9857830 unfound (0.000%); 
    nagios$ echo $?
    1
    nagios$

# Installation

Just type `make install` in the same directory as this README file can be found


[ceph]: http://www.ceph.com
[cephx]: http://ceph.com/docs/master/rados/operations/authentication/
