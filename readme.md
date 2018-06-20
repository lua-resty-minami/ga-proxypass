Name
====
lua-resty-minami/google_analytics : Openresty library to build a google_analytics webserver


Table of Contents
=================
* [Name](#name)
* [Status](#status)
* [Description](#description)
* [Synopsis](#synopsis)
* [Author](#author)
* [Copyright and License](#copyright-and-license)
* [See Also](#see-also)
* [According](#according)


Status
======
This library is considered passed testing and may change without notice.

[Back to TOC](#table-of-contents)


Description
===========
This library requires a Openresty build, please take [here](https://sometimesnaive.org/article/71) as reference.

[Back to TOC](#table-of-contents)


Synopsis
========
```nginx
# nginx.conf

# library folder 'resty' dir
lua_package_path "/path/to/conf/?.lua;;";



# website.conf
location / {
    ...
    
    access_by_lua_file  /path/to/conf/access.lua;
    
    if ($cookie_stats = "1") {
        rewrite ^/(.*)$ /$1 break;
        post_action @google_analytics;
    }
}

location @google_analytics {
    internal;
    
    resolver 8.8.8.8 ipv6=off;
    
    proxy_http_version          1.1;
    proxy_method                GET;
    proxy_set_header            User-Agent $http_user_agent;
    proxy_pass_request_headers  off;
    proxy_pass_request_body     off;
    # modify 'UA-111111111-1' to your google_analytics website number
    proxy_pass                  https://www.google-analytics.com/collect?v=1&t=pageview&tid=UA-111111111-1&cid=$cookie_cid&uip=$remote_addr&dh=$host&dp=$request_uri&dr=$http_referer&z=$msec;
}
```

```lua
-- access.lua

-- as to the following ".../"
-- you should modify it to absolute directory of root directory of your website
local request_filename = ".../" .. ngx.var.uri
```

[Back to TOC](#table-of-contents)


Author
======
Minami (Nanqinlang) (南琴浪) <https://sometimesnaive.org>

[Back to TOC](#table-of-contents)


Copyright and License
=====================
personal work, for non-profit, Copyright (C) 2018 All rights reserved.

This library's License: GPL v3.

[Back to TOC](#table-of-contents)


See Also
========
* the ngx_lua module: http://wiki.nginx.org/HttpLuaModule

[Back to TOC](#table-of-contents)


According
========
https://sometimesnaive.org/article/76
