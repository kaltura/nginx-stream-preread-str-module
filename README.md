# Nginx Stream Pre-read Str Module

Nginx module for reading a string "header" from a stream connection.
The string is consumed up to a predefined delimiter, and is exposed as a stream variable.

## Build

![Build Status](https://github.com/kaltura/nginx-stream-preread-str-module/actions/workflows/ci.yml/badge.svg)

To link statically against nginx, cd to nginx source directory and execute:

    ./configure --add-module=/path/to/nginx-stream-preread-str-module --with-stream

To compile as a dynamic module (nginx 1.9.11+), use:

    ./configure --add-dynamic-module=/path/to/nginx-stream-preread-str-module --with-stream

In this case, the `load_module` directive should be used in nginx.conf to load the module.

## Configuration

### Sample configuration

```
# Dynamic TCP -> TLS proxy
stream {
    server {
        listen 5432;

        preread_str_delim '\n';

        proxy_pass $preread_str;
        proxy_ssl on;

        resolver 8.8.8.8;
    }
}
```

### Configuration directives

#### preread_str_delim
* **syntax**: `preread_str_delim delim;`
* **default**: ``
* **context**: `stream, server`

Consumes data from the incoming connection up to the provided delimiter.
The consumed data is not seen by any content phase handlers (for example, `proxy_pass` will not proxy it),
but can be accessed using the `$preread_str` variable.
If the delimiter is not found after reading `preread_buffer_size` bytes (defined in nginx stream core), the connection is dropped.

## Embedded Variables

* `$preread_str` - the string that was read from the stream connection.

## Copyright & License

All code in this project is released under the [AGPLv3 license](http://www.gnu.org/licenses/agpl-3.0.html) unless a different license for a particular library is specified in the applicable library path.

Copyright © Kaltura Inc. All rights reserved.
