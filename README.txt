
lets-encrypt-nginx; script for configuring nginx and lets encrypt.

USAGE:

  lets-encrypt-nginx [ <config-file> ] [--sites <sites-list>] [options]

  config-file defaults to ./lengx.conf; must be first argument when set
  sites-list defaults to ./sites.conf

See CONFIG and SITES below for a description of those two files. 

Options:

  --site <sitename>

    Only process site named 'sitename'

  --no-write-nginx, --no-write-certbot, --no-write

    Don't write nginx config, certbot config, or either. Assembles the config, 
    but skips the bit where it would be writen.

  --no-run-nginx, --no-run-certbot, --no-run
  
    Don't run the nginx site-enabling command,  don't run the certbot cert-
    issuing command, or don't run either. 

  --noclobber-nginx, --noclobber-certbot, --noclobber-both

    Disable overwriting of existing nginx configs, certbot configs, or both. Will 
    print a warning instead.

  --overwrite-certs

    (attempt to) get certs issued even if they already exist. Will probably involve
    using certbot's interactive interface. 

  --skip-nginx --skip-certbot

    Skip all nginx configuring, or skip all certbot configuration. Calling with
    both is permitted, but pointless.


  --dump-nginx, --dump-certbot

    Dump nginx config to stdout, or dump certbot config to stdout (rather than
    writing to files). Not affected by --no-write*

  --dump-config, --dump-sites

    Dump the configuration, or site definitions, to stdout and exit. Config is 
    dumped in the format of a valid config file, but sites are not.
    
CONFIG

The config file is a series of key=value pairs, and the keys exactly match those
used in the %config hash internally. Check the top of the script for currently-
known ones, and use --dump-config to produce the file. 

Unsupported config keys cause a warning to be displayed, but don't prevent execution.
Lines beginning '#' are skipped as comments.

See ./lengx.conf.example for all available config options.

SITES

Each site is defined, at minimum, by a single space-separated line of domain names. 
The first item in this list is taken as the name of the site, and  options may be 
set on successive lines that begin with spaces.

See ./sites.conf.example for an example.

DEBUGGING

Set the environment variable 'DEBUG' to anything to have helpful(!) messages printed to stderr:

    DEBUG=1 lets-encrypt-nginx --no-run --no-write 

is a likely invocation here.

There are two canonical repositories for this, each with equal status:

  https://git.avi.co/avi/lets-encrypt-nginx
  https://bitbucket.org/BigRedS/lets-encrypt-nginx

