## Squid.conf from Gentoo Linux

```sh
# Bind to a ip and port
http_port 10.0.2.1:3128

# Standard configuration
hierarchy_stoplist cgi-bin ?
acl QUERY urlpath_regex cgi-bin \?
no_cache deny QUERY

# Add basic access control lists
acl all src 0.0.0.0/0.0.0.0
acl manager proto cache_object
acl localhost src 127.0.0.1/255.255.255.255

# Add who can access this proxy server
acl localnet src 10.0.0.0/255.255.0.0

# And ports
acl SSL_ports port 443
acl Safe_ports port 80
acl Safe_ports port 443
acl purge method PURGE

# Add access control list based on regular
# expressions within urls
acl archives urlpath_regex "/etc/squid/files.acl"
acl url_ads url_regex "/etc/squid/banner-ads.acl"

# Add access control list based on time and day
acl restricted_weekdays time MTWHF 8:00-17:00
acl restricted_weekends time A 8:00-13:00

acl CONNECT method CONNECT

#allow manager access from localhost
http_access allow manager localhost
http_access deny manager

# Only allow purge requests from localhost
http_access allow purge localhost
http_access deny purge

# Deny requests to unknown ports
http_access deny !Safe_ports

# Deny CONNECT to other than SSL ports
http_access deny CONNECT !SSL_ports

# My own rules

# Add a page do be displayed when
# a banner is removed
deny_info NOTE_ADS_FILTERED url_ads

# Then deny them
http_access deny url_ads

# Deny all archives
http_access deny archives

# Restrict access to work hours
http_access allow localnet restricted_weekdays
http_access allow localnet restricted_weekends

# Deny the rest
http_access deny all
```

