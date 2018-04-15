## Release x.y.z

This release introduces backward-incompatibilities.

The following variables are removed from the role.

* `ntpd_role`
* `ntpd_upstreams`
* `ntpd_pools`

The template for `ntp.conf` is now empty by default. Define `ntpd_config` and
build your own configuration.

## Release 1.4.0

* 1471b51 [feature][bugfix] remove obsolete modules and attributes, support FreeBSD 11.1 (#3)

## Release 1.3.0

* 7c6934f [feature] make ntpd_leapfile optional (#25)
* 38166d2 [bugfix] QA (#23)

## Release 1.2.1

* d0add6e [bugifx] set ntpd_sync_on_start="YES" on FreeBSD (fixes #16)

## Release 1.2.0

* 59d9be7 add a description about ntpd_supports_pool
* 813aae1 fix meta; add xenial
* ce9b4c5 remove jinja2 templating delimiters to fix ansible warning
* 4200405 validate ntp.conf
* 1287000 [feature] introduce ntpd_pools (fix #17, fix #18)
* 7319def [feature] support Ubuntu 16.04
* 2b3243c QA
* 209a488 update CentOS to 7.3

## Release 1.1.3

* QA only

## Release 1.1.2

* install libselinux-python when SELinux is enabled, fixes #8

## Release 1.1.1

* update README

## Release 1.1.0

* support centos
* support ubuntu

## Release 1.0.0

* Initail release
