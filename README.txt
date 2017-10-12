
lets-encrypt-nginx; script for configuring nginx and lets encrypt.

USAGE:

  lets-encrypt-nginx [ <config-file> ] [--sites <sites-list>] [options]

  config-file defaults to ./lengx.conf; must be first argument when set
  sites-list defaults to ./sites.conf

See CONFIG and SITES below for a description of those two files. 

Options:

  --nocleartext

    Don't insert config to cause Nginx to listen for cleartext HTTP (on port
    80 by default). This makes nginx an SSL-terminator only.

  --no-write-nginx, --no-write-certbot, --no-write

    Don't write nginx config, certbot config, or either. Assembles the config, 
    but skips the bit where it would be writen.


  --no-run-nginx, --no-run-certbot, --no-run
  
    Don't run the nginx site-enabling command,  don't run the certbot cert-
    issuing command, or don't run either. 


  --clobber-nginx, --clobber-certbot, --clobber-both

    Overwrite existing nginx configs, certbot configs, or both. Default is to
    print a warning and skip the site.


  --skip-nginx --skip-certbot

    Skip all nginx configuring, or skip all certbot configuration. Calling with
    both is permitted, but pointless.


  --dump-nginx, --dump-certbot

    Dump nginx config to stdout, or dump certbot config to stdout (rather than
    writing to files). Not affected by --no-write*

  --dump-config, --dump-sites

    Dump the configuration, or site definitions, to stdout before doing any 
    processing. Config is dumped in the format of a valid config file, but sites
    are not.
    
CONFIG

The config file is a series of key=value pairs, and the keys exactly match those
used in the %config hash internally. Check the top of the script for currently-
known ones, and use --dump-config to produce the file. 

Unsupported config keys cause a warning to be displayed, but don't prevent execution.
Lines beginning '#' are skipped as comments.

There should be an example at ./lengx.conf.example

SITES

Each site is defined, at minimum, by a single space-separated line of domain names. 
The first item in this list is taken as the name of the site. Options may be set on 
successive lines that begin with spaces; currently supported options are:

  auto_www - if set to 'on', automatically add the 'www' subdomain to all names. Set
             to 'off' to disable.
  backend  - literal string used as the argument to nginx's proxy_pass

The first site in the file may be named '_default' to set defaults for all successive 
sites. Lines beginning '#' are skipped as comments.

There should be an example at ./sites.conf.example

DEBUGGING

Set the environment variable 'DEBUG' to anything to have helpful(!) messages printed to stderr:

    DEBUG=1 lets-encrypt-nginx --no-run --no-write 

is a likely invocation here.

