
Fail2Ban Role
==============
[![Build Status](https://travis-ci.org/Blue-Bag/ansible-f5-fail2ban.svg?branch=main)](https://travis-ci.org/Blue-Bag/ansible-f5-fail2ban)

Install and configure Fail2Ban.

Role Variables
--------------------
See default.yml for commented vars.

Usage
---------

If you want to provide a list of allowed set of ips
Add them to the ignoreip list:

You can use list filters to build up the list in a pre task.

    - name: join ignore ip lists
       set_fact:
         fail2ban_config_ignoreip: "{{ myslistofips|union(myotherlistofips) }}"

or just set them in vars (e.g. Allowlist the Newrelic ips)

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

Notifications
the default is to log and email a notigication
This can get quite noisy with no required action so to tun off mailing and just log and ban the event changes

``` %(action_mwl)s
to 
``` %(action_)s

Notes & parameters
ignoreip: This parameter identifies IP addresses that should be ignored by the banning system. By default, this is just set to ignore traffic coming from the machine itself, so that you don’t fill up your own logs or lock yourself out.

bantime: This parameter sets the length of a ban, in seconds. The default is 10 minutes.

findtime: This parameter sets the window that Fail2ban will pay attention to when looking for repeated failed authentication attempts. The default is set to 10 minutes, which means that the software will count the number of failed attempts in the last 10 minutes.

maxretry: This sets the number of failed attempts that will be tolerated within the findtime window before a ban is instituted.

backend: This entry specifies how Fail2ban will monitor log files. The setting of auto means that fail2ban will try pyinotify, then gamin, and then a polling algorithm based on what’s available. inotify is a built-in Linux kernel feature for tracking when files are accessed, and pyinotify is a Python interface to inotify, used by Fail2ban.

usedns: This defines whether reverse DNS is used to help implement bans. Setting this to “no” will ban IPs themselves instead of their domain hostnames. The warn setting will attempt to look up a hostname and ban that way, but will log the activity for review.

destemail: This is the address that will be sent notification mail if configured your action to mail alerts.

sendername: This will be used in the email from field for generated notification emails

banaction: This sets the action that will be used when the threshold is reached. This is actually a path to a file located in /etc/fail2ban/action.d/ called iptables-multiport.conf. This handles the actual iptables firewall manipulation to ban an IP address. We will look at this later.

mta: This is the mail transfer agent that will be used to send notification emails.

protocol: This is the type of traffic that will be dropped when an IP ban is implemented. This is also the type of traffic that is sent to the new iptables chain.

chain: This is the chain that will be configured with a jump rule to send traffic to the fail2ban funnel.

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
