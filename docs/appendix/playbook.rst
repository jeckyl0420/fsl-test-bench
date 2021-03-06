.. -*- mode: rst -*-

.. _appendix-playbook:

.. _all-in-one.yml: https://github.com/fabaff/fsl-test-bench/blob/master/all-in-one.yml
.. _git repository: https://github.com/fabaff/fsl-test-bench

Playbook
========

The **all-in-one.yml** playbook contains all items which are installed on the
FSL Test bench. This should give the reader a bit of an inside view of the
creation workflow of the Fedora Security Lab Test bench. This playbook calls
every include playbook which contains the tasks for the service it represents.::

    # This playbook contains tasks to perform on a fresh Fedora installation to
    # create a Fedora Security Lab Test bench. 
    #
    # Copyright (c) 2013 Fabian Affolter <fabian@affolter-engineering.ch>
    #
    # Licensed under CC BY 3.0. All rights reserved.
    #
    # Usage: sudo ansible-playbook all-in-one.yml -f 10
    # 
    ---
    - hosts: fsl-tb
      user: root
      vars_files:
        - variables/application-versions.yml
        - variables/sensitive.yml

      tasks:
    # Common tasks
    ################################################################################
        - include: tasks/preparation.yml
        - include: tasks/motd.yml
        - include: tasks/hosts.yml
        - include: tasks/users.yml

    # FSL Test bench specific stuff
    ################################################################################
        - include: tasks/web-servers/lighttpd.yml
        - include: tasks/web-interface.yml

    # Services
    ################################################################################
    ## Web servers
        - include: tasks/web-servers/nginx.yml
        - include: tasks/web-servers/tomcat.yml
        - include: tasks/web-servers/pywebserve.yml
        - include: tasks/web-servers/nodejs.yml
        - include: tasks/web-servers/mongoose.yml
        - include: tasks/web-servers/darkhttpd.yml
    ## Database server
        - include: tasks/db-servers/mysql.yml
    ## FTP server
        - include: tasks/ftp-servers/vsftpd.yml
    ## Misc servers
        - include: tasks/misc-servers/openssh.yml
        - include: tasks/misc-servers/dropbear.yml
        - include: tasks/misc-servers/tftp.yml
        - include: tasks/misc-servers/telnet.yml
        - include: tasks/misc-servers/cups.yml
        - include: tasks/misc-servers/openvpn-static.yml
        - include: tasks/misc-servers/xrdp.yml
        - include: tasks/misc-servers/mosquitto.yml
    ## File servers
        - include: tasks/file-servers/samba.yml
        - include: tasks/file-servers/nfs.yml  # Needs manual start
    ## Mail server
        - include: tasks/mail-servers/postfix.yml
        - include: tasks/mail-servers/dovecot.yml

    # Helpers
    ################################################################################
        - include: tasks/helpers/log-system.yml
        - include: tasks/helpers/phpinfo.yml
    # Shell Detector is not able to handle the present amount of files. Memory?
    #    - include: tasks/helpers/php-shell-detector.yml
        - include: tasks/helpers/linfo.yml
        - include: tasks/helpers/phpmyadmin.yml

    # Common Gateway Interface (CGI)
    ################################################################################
        - include: tasks/cgi/cgi.yml
    ## C (default)
        - include: tasks/cgi/time.yml
    ## Python
        - include: tasks/cgi/time-py.yml
    ## Bash
        - include: tasks/cgi/env-sh.yml
        - include: tasks/cgi/system-sh.yml
    ## Perl
        - include: tasks/cgi/time-pl.yml

    # Vulnerable Web Application
    ################################################################################
        - include: tasks/apps/sqlol.yml
        - include: tasks/apps/sqli.yml
        - include: tasks/apps/bwapp.yml
        - include: tasks/apps/dvwa.yml
        - include: tasks/apps/hackademic.yml
        - include: tasks/apps/xssed.yml

    # Honeypots
    ################################################################################
        - include: tasks/honeypots/honeyd.yml

    # Shells
    ################################################################################
        - include: tasks/shells/b374k.yml
        - include: tasks/shells/dnashell.yml
        - include: tasks/shells/phpshell.yml
        - include: tasks/shells/ajaxshell.yml

    # Common tasks
    ################################################################################
        - include: tasks/cleanup.yml

      handlers:
       - include: handlers/system.yml
       - include: handlers/services.yml

The `all-in-one.yml`_ playbook can be found in the FSL Test bench
`git repository`_.
