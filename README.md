# ansible-role-ntpd

Configure ntpd from ntp.org

# Requirements

None

# Role Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `ntpd_service` | service name of `ntpd` | `{{ __ntpd_service }}` |
| `ntpd_conf` | path to `ntp.conf` | `{{ __ntpd_conf }}` |
| `ntpd_config` | content of `ntp.conf` | `""` |
| `ntpd_db_dir` | path to directory where `ntpd.leap-seconds.list` is kept | `{{ __ntpd_db_dir }}` |
| `ntpd_script_dir` | directory to keep support script. this must be included in PATH environment variable | `{{ __ntpd_script_dir }}` |
| `ntpd_leapfile` | path to `leap-seconds.list` If the value is empty string, `leapfile` setting is not added to `ntp.conf` | `{{ ntpd_db_dir }}/leap-seconds.list` |
| `ntpd_package` | package name of `ntpd` | `{{ __ntpd_package }}` |
| `ntpd_driftfile` | path to `ntp.drift` | `{{ ntpd_db_dir }}/ntp.drift` |
| `ntpd_leap_seconds_url` | URL of `leap-seconds.list`. If the value is empty string, the file is not downloaded. | `https://www.ietf.org/timezones/data/leap-seconds.list` |


## `ntpd_supports_pool`

`ntpd_supports_pool` is a fact which is `true` when `ntpd >= 4.2.7` and
`false` when `ntpd < 4.2.7`.

## Debian

| Variable | Default |
|----------|---------|
| `__ntpd_service` | `ntp` |
| `__ntpd_conf` | `/etc/ntp.conf` |
| `__ntpd_db_dir` | `/var/lib/ntp` |
| `__ntpd_package` | `ntp` |
| `__ntpd_script_dir` | `/usr/bin` |

## FreeBSD

| Variable | Default |
|----------|---------|
| `__ntpd_service` | `ntpd` |
| `__ntpd_conf` | `/etc/ntp.conf` |
| `__ntpd_db_dir` | `/var/db/ntp` |
| `__ntpd_script_dir` | `/usr/local/bin` |

## RedHat

| Variable | Default |
|----------|---------|
| `__ntpd_service` | `ntpd` |
| `__ntpd_conf` | `/etc/ntp.conf` |
| `__ntpd_db_dir` | `/var/lib/ntp` |
| `__ntpd_package` | `ntp` |
| `__ntpd_script_dir` | `/usr/bin` |

# Dependencies

None

# Example Playbook

```yaml
- hosts: localhost
  roles:
    - ansible-role-ntpd
  vars:
    upstreams:
      - time1.google.com
      - time2.google.com
      - time3.google.com
      - time4.google.com
    # Servers provided by DNS round robin must be added to `pools` because
    # special treatment for restrictions is required.  ntpd >= 4.2.7 supports `pool`
    # directive and a delegate pool name can be used.  see
    # http://support.ntp.org/bin/view/Support/ConfiguringNTP#Section_6.10 for detail
    pools:
      - 0.pool.ntp.org
      - 1.pool.ntp.org
      - 2.pool.ntp.org
      - 3.pool.ntp.org
    ntpd_config: |
      {% for s in upstreams %}
      server {{ s }} iburst
      restrict {{ s }} nomodify notrap noquery
      {% endfor %}

      {% if ntpd_supports_pool %}
      restrict default ignore
      restrict -6 default ignore
      pool {{ pools | first }} iburst
      restrict source nomodify noquery notrap
      {% else %}
      restrict default -4 nomodify nopeer noquery notrap
      restrict default -6 nomodify nopeer noquery notrap
      {% for p in pools %}
      server {{ p }} iburst
      {% endfor %}
      {% endif %}

      restrict localhost
      leapfile "{{ ntpd_leapfile }}"
      driftfile "{{ ntpd_driftfile }}"
```

# License

```
Copyright (c) 2016 Tomoyuki Sakurai <y@trombik.org>

Permission to use, copy, modify, and distribute this software for any
purpose with or without fee is hereby granted, provided that the above
copyright notice and this permission notice appear in all copies.

THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL WARRANTIES
WITH REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED WARRANTIES OF
MERCHANTABILITY AND FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR
ANY SPECIAL, DIRECT, INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES
WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN AN
ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF
OR IN CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.
```

# Author Information

Tomoyuki Sakurai <y@trombik.org>
