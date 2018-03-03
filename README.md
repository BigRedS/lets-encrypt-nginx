# lengx: Let's Encrypt Nginx

An easy way of managing Lets Encrypt certs in an Nginx https-terminator.

Lengx reads its own config files and uses that to write nginx and Let's 
Encrypt certbot config, then invokes certbot to retrieve those certs.

It's intentionally pretty simple; this is intended to make large and simple 
installations into trivial ones, not to simplify complex webserver configs.

## Installation

So far, everywhere this is installed, it's a simple git checkout into 

    /root/lets-encrypt-nginx

While it's in this relatively-unstable state, always do a `git pull` before 
running it.

## Configuration

There are two config files: 

* `lengx.conf` contains details of the nginx and certbot installation, and 
  system-wide settings. An example file at `./lengx.conf.example` is a real-
  life one-line example, and `./lengx.conf.complete` documents every available
  option.

* `sites.conf` contains definitions of sites to configure - their domain names,
  backend details and suchlike. `./sites.conf.example` is a real-life example 
  of this file, and `./sites.conf.complete` documents every available option.


When lengx is invoked, it assumes lengx.conf is present at `./lengx.conf`, and 
the sites.conf must be passed using the `--sites` option:

    ./lengx --sites ./sites.conf

On systems with configurations for several environments, or several customers, 
conventionally there will be a `./sites` directory containing a series of sites
config files, but lengx itself doesn't seek these out.


A simple site config is:

    example.co.uk www.example.co.uk
      backend http://10.0.0.2:80

This configures a site named 'example.co.uk' (the first domain in the list is
used as the site's name internally), and configuration options are on successive
indented lines.

This will set up an nginx server (in /etc/nginx/sites-available/example.co.uk)
listening for example.co.uk and www.example.co.uk, and proxying all requests
back to http://10.0.0.2:80. A Let's Encrypt certificate will be issued for those
two names, too, and used on the SSL vhost.



It is assumed that Nginx has a default cleartext server which simply redirects
all requests to their https equivalents. In this instance, adding `nginx_ssl off`
to the site definition:

    example.co.uk www.example.co.uk
      backend http://10.0.0.2:80
      nginx_ssl off

means that the Nginx config for example.co.uk will only listen for HTTPS (on 
port 443); requests for http will instead get through to the default server and
be redirected to https.



Finally, lengx will parse each existing Nginx config file, and on finding a 
line `#LENGX: SKIP` will skip writing that site's Nginx config, effectively 
treating it as read-only, and carry on as if it were succefully written-to.
This is intended primarily to avoid clobbering debugging/testing config.

