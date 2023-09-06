<div align="center">
<p>

# Naett.c3 - A tiny binding for [naett - C HTTP/HTTPS library](https://github.com/erkkah/naett) and [C3 Programming Language](https://c3-lang.org/)

</p>
</div>

**Naett.c3** is a [C3](https://c3-lang.org/) binding for the [naett - Tiny cross-platform HTTP / HTTPS client library in C. ](https://github.com/erkkah/naett)

## Installation
Clone the original C naett library and produce a static library named `libnaett`, your final product should be `libnaett.a` and be located within the desired platform within `naett.c3l`.

Add naett.c3 as your dependency within `project.json` as standard.

## Example

```c
import naett;
import std::thread;
import std::io;

fn int main(int argc, char** argv) {
    if (argc < 2) {
        io::printn("Expected URL argument");
        return 1;
    }

    char* url = argv[1];
    naett::init(null);

    naett::Req* req = naett::request(url, naett::method("GET"), naett::header("accept", "*/*"));
    naett::Res* res = naett::make(req);

    while (!naett::complete(res)) {
        thread::sleep_ms(100);
    }

    int status = naett::getStatus(res);

    if (status < 0) {
        io::printf("Request failed: %d\n", status);
        return 1;
    }

    int bodyLength = 0;
    ZString body = (ZString)naett::getBody(res, &bodyLength);

    io::printf("Got a %d, %d bytes of type '%s':\n\n", naett::getStatus(res), bodyLength, (ZString)naett::getHeader(res, "Content-Type"));
    io::printf("%.100s\n...\n", body);

    naett::close(res);
    naett::free(req);

    return 0;
}

```

## Credits
All original work for naett goes to its respective owner.
