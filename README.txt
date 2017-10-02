
lets-encrypt-nginx; script for configuring nginx and lets encrypt.

USAGE:

    lets-encrypt-nginx <options> --domains [path]

  or

    lets-encrypt-nginx --generate-tlds-file


Where [path] is to a file containing a list of domains, one per line.

OPTIONS:
(default/current values in brackets)

  --tlds-list-file [path]
      file containing a list of TLDs; see 'TLDS' below. 
      (./tlds.txt)

  --get-tlds-script [path]
      path to script for generating tlds-list-file 
      (./get-tld-list)

  --clobber-certbot
  --clobber-nginx
      Silently overwrite existing config for certbot and/or nginx respectively. 
      default is to print a warning and not overwrite.

  --skip-certbot
  --skip-nginx
      Skip certbot or Nginx configuration, respectively.

  --dump-certbot
  --dump-nginx
      Rather than create/enable config files, just print all generated config to
      stdout. Overriden by the 'skip' equivalents.
      dump-certbot is probably useless outside of debugging.


  nginx:

    --nginx-ssl-socket [port or ip-address:port]
        argument to nginx's 'listen' directive, for SSL connections. Do not
        include trailing 'ssl' at the end; *just* the socket (443)

    --nginx-cleartext-socket [port or ip-address:port]
        same, for cleartext (80)

    --nginx-nocleartext 
        default is to add a 'listen 80' line to the nginx config. Set this to skip
        that

    --nginx-backend [url]
        URL to proxied-to backend (http://127.0.0.1)

    --nginx-log-path-prefix [path]
        Used to create error and access log paths for the nginx config. The string
        '%SITENAME%' is replaced with the site name, and then '_error.log' or 
        '_access.log' is appended.
        (/var/log/nginx/%SITE_NAME%)

    --nginx-config-dir [path]
        path to nginx sites-available directory (/etc/nginx/sites-available/);

    --nginx-enable-site [command]
        thing to execute to enable the config that's presumably been written to 
        sites-avilable. Passed the file basename as its only argument; expects to
        have something like this installed: https://github.com/perusio/nginx_ensite
        Disable this by passing `/bin/true`
        (nginx_ensite)

    --nginx-include-file [path]
        path to a file to include in the Nginx config. Is inserted verbatim, at the 
        end. It probably wants every line to be indented by one tab, for neatness' 
        sake
        ()
        
  certbot:

    --certbot-config-dir [path]
        directory in which certbot config files are created 
       (/etc/letsencrypt/configs/)

    --certbot-key-size [bits]
        number of bits for the private key (2048)
    
    --certbot-server-url [url]
        URL to the certbot server (https://acme-v01.api.letsencrypt.org/directory)
    
    --certbot-email [email-address]
        Email address for the Let's Encrypt cert (letsencrypt@my-company.net)
    
    --certbot-webroot [path]
        Webroot directory (/var/www/letsencrypt/)
    
    --certbot-binary [path]
        Path to certbot itself (/opt/letsencrypt/letsencrypt-auto)


TLDS

In order to determine a reasonable 'site name', this script needs to know which portion 
of a domain name is TLD or Second-level domain (SLD), and which portion is the part that
was actually registered by the customer.

It uses a file tlds.txt (specified with --tlds-list-file) as reference, which is a list,
one-item-per-line, of every IANA TLD, and every country-specific second-level domain 
(.co.uk, for example). Each domain is checked against this list, and the longest-matching
string is taken to deduce the registered domain. "mycompany.co.uk" would match both '.co.uk' 
and '.uk.'; .co.uk would be selected as the longest-match, and so 'mycompany' would be the 
name of the site, and 'mycompany.com' seen as related.

This file can be created with the included get-tld-list script:

    get-tld-list > ./tlds.txt

or by invoking this with --generate-tlds-file:

   lets-encrypt-nginx --generate-tlds-file

which will silently overwrite ./tlds.txt (or whatever's set with --tlds-list-file).

