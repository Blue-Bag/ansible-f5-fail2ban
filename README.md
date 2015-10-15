
Fail2Ban Role
==============
Install and configure Fail2Ban.

Role Variables
--------------------
See deafult.yml for commented vars.

Usage
---------

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

Note:
Got an error in startup of fail2ban: No 'host' group in '# Option: ignoreregex'
due to indentation of lines!! make sure all comments have no indentation!!

If you are monitoring a service like exim4 or proftp - their logs have to exist
If they don't then the startup will fail.
There is a check in the task to touch the exim4 reject log since that is not always
present even if exim4 is running.

e.g.



     # Option: ignoreregex
       # Notes.: regex to ignore. If this regex matches, the line is ignored.
       # Values: TEXT
       ignoreregex =
   should have been (no indentation)

    # Option: ignoreregex
    # Notes.: regex to ignore. If this regex matches, the line is ignored.
    # Values: TEXT
    ignoreregex =

License
-------
BSD

Author Information
------------------
refs:

 - http://www.pontikis.net/blog/fail2ban-install-config-debian-wheezy
 - http://serverfault.com/questions/415040/permanent-block-of-ip-after-n-retries-using-fail2ban
 - http://stuffphilwrites.com/2013/03/permanently-ban-repeat-offenders-fail2ban/
Testing:
http://www.the-art-of-web.com/system/fail2ban-howto/
see other Galaxy roles:
 - https://github.com/resmo/ansible-role-fail2ban
 - https://github.com/Oefenweb/ansible-fail2ban

 Could install from source: https://extremeshok.com/1759/centos-6-rhel-6-install-the-latest-fail2ban-from-source/
