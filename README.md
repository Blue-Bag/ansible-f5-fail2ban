Fail2 Ban Role
========

Install and configure Fail2Ban.

Role Variables
--------------



Usage
-----

If you want to white list a set of ips
Add them to the ignoreip list:

You can use list filters to build up the list in a pre task.

- name: join ignore ip lists
   set_fact:
     fail2ban_config_ignoreip: "{{ myslistofips|union(myotherlistofips) }}"

or just set them in vars (e.g. Whitelist the Newrelic ips)

fail2ban_config_ignoreip:
  - "127.0.0.1/8"
  - "50.31.164.139"
  - "50.112.95.211"
  - "54.247.188.179"
  - "54.248.250.232"
  - "54.251.34.67"
  - "184.73.237.85"



License
-------

BSD

Author Information
------------------
refs:
http://www.pontikis.net/blog/fail2ban-install-config-debian-wheezy
http://serverfault.com/questions/415040/permanent-block-of-ip-after-n-retries-using-fail2ban
http://stuffphilwrites.com/2013/03/permanently-ban-repeat-offenders-fail2ban/

see Galaxy roles:
 https://github.com/resmo/ansible-role-fail2ban
 https://github.com/Oefenweb/ansible-fail2ban