# Async Python Web Frameworks comparison

https://klen.github.io/py-frameworks-bench/
----------
#### Updated: 2021-05-31

[![benchmarks](https://github.com/klen/py-frameworks-bench/actions/workflows/benchmarks.yml/badge.svg)](https://github.com/klen/py-frameworks-bench/actions/workflows/benchmarks.yml)
[![tests](https://github.com/klen/py-frameworks-bench/actions/workflows/tests.yml/badge.svg)](https://github.com/klen/py-frameworks-bench/actions/workflows/tests.yml)

----------

This is a simple benchmark for python async frameworks. Almost all of the
frameworks are ASGI-compatible (aiohttp and tornado are exceptions on the
moment).

The objective of the benchmark is not testing deployment (like uvicorn vs
hypercorn and etc) or database (ORM, drivers) but instead test the frameworks
itself. The benchmark checks request parsing (body, headers, formdata,
queries), routing, responses.

## Table of contents

* [The Methodic](#the-methodic)
* [The Results](#the-results-2021-05-31)
    * [Accept a request and return HTML response with a custom dynamic header](#html)
    * [Parse uploaded file, store it on disk and return a text response](#upload)
    * [Parse path params, query string, JSON body and return a json response](#api)
    * [Composite stats ](#composite)



<img src='https://quickchart.io/chart?width=800&height=400&c=%7Btype%3A%22bar%22%2Cdata%3A%7Blabels%3A%5B%22blacksheep%22%2C%22muffin%22%2C%22falcon%22%2C%22starlette%22%2C%22emmett%22%2C%22sanic%22%2C%22fastapi%22%2C%22aiohttp%22%2C%22tornado%22%2C%22quart%22%2C%22django%22%5D%2Cdatasets%3A%5B%7Blabel%3A%22num%20of%20req%22%2Cdata%3A%5B553635%2C490470%2C450315%2C373035%2C346905%2C322590%2C280140%2C226770%2C130230%2C115125%2C66840%5D%7D%5D%7D%7D' />

## The Methodic

The benchmark runs as a [Github Action](https://github.com/features/actions).
According to the [github
documentation](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners)
the hardware specification for the runs is:

* 2-core vCPU (Intel® Xeon® Platinum 8272CL (Cascade Lake), Intel® Xeon® 8171M 2.1GHz (Skylake))
* 7 GB of RAM memory
* 14 GB of SSD disk space
* OS Ubuntu 20.04

[ASGI](https://asgi.readthedocs.io/en/latest/) apps are running from docker using the gunicorn/uvicorn command:

    gunicorn -k uvicorn.workers.UvicornWorker -b 0.0.0.0:8080 app:app

Applications' source code can be found
[here](https://github.com/klen/py-frameworks-bench/tree/develop/frameworks).

Results received with WRK utility using the params:

    wrk -d15s -t4 -c64 [URL]

The benchmark has a three kind of tests:

1. "Simple" test: accept a request and return HTML response with custom dynamic
   header. The test simulates just a single HTML response.

2. "Upload" test: accept an uploaded file and store it on disk. The test
   simulates multipart formdata processing and work with files.

3. "API" test: Check headers, parse path params, query string, JSON body and return a json
   response. The test simulates an JSON REST API.


## The Results (2021-05-31)

<h3 id="html"> Accept a request and return HTML response with a custom dynamic header</h3>
<details open>
<summary> The test simulates just a single HTML response. </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.6` | 19594 | 2.81 | 4.14 | 3.23
| [muffin](https://pypi.org/project/muffin/) `0.78.0` | 17283 | 3.07 | 4.78 | 3.67
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 16006 | 3.29 | 5.19 | 3.96
| [emmett](https://pypi.org/project/emmett/) `2.2.1` | 14099 | 3.77 | 5.90 | 4.50
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 14076 | 3.66 | 5.90 | 4.51
| [fastapi](https://pypi.org/project/fastapi/) `0.65.1` | 9867 | 5.02 | 8.74 | 6.45
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 9238 | 5.39 | 9.10 | 6.94
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 7755 | 8.27 | 8.32 | 8.26
| [quart](https://pypi.org/project/quart/) `0.15.1` | 3551 | 18.48 | 19.17 | 18.00
| [tornado](https://pypi.org/project/tornado/) `6.1` | 3420 | 18.69 | 18.80 | 18.71
| [django](https://pypi.org/project/django/) `3.2.3` | 1875 | 33.95 | 37.66 | 34.16


</details>

<h3 id="upload"> Parse uploaded file, store it on disk and return a text response</h3>
<details open>
<summary> The test simulates multipart formdata processing and work with files.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.6` | 6252 | 7.82 | 13.82 | 10.25
| [muffin](https://pypi.org/project/muffin/) `0.78.0` | 4799 | 10.21 | 17.96 | 13.30
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 4477 | 10.94 | 19.30 | 14.27
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 3800 | 13.39 | 22.29 | 16.94
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 2627 | 29.69 | 31.85 | 24.31
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 2427 | 26.34 | 26.43 | 26.36
| [fastapi](https://pypi.org/project/fastapi/) `0.65.1` | 2312 | 21.44 | 37.78 | 27.63
| [tornado](https://pypi.org/project/tornado/) `6.1` | 2309 | 27.67 | 27.81 | 27.70
| [quart](https://pypi.org/project/quart/) `0.15.1` | 1911 | 33.31 | 33.86 | 33.48
| [emmett](https://pypi.org/project/emmett/) `2.2.1` | 1603 | 36.73 | 45.41 | 39.88
| [django](https://pypi.org/project/django/) `3.2.3` | 1026 | 62.15 | 69.63 | 62.28


</details>

<h3 id="api"> Parse path params, query string, JSON body and return a json response</h3>
<details open>
<summary> The test simulates a simple JSON REST API endpoint.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.6` | 11063 | 4.55 | 7.57 | 5.75
| [muffin](https://pypi.org/project/muffin/) `0.78.0` | 10616 | 4.65 | 8.04 | 6.00
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 10215 | 4.88 | 8.44 | 6.23
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 8166 | 6.08 | 10.54 | 7.80
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 7791 | 6.31 | 10.83 | 8.22
| [emmett](https://pypi.org/project/emmett/) `2.2.1` | 7425 | 6.68 | 11.20 | 8.71
| [fastapi](https://pypi.org/project/fastapi/) `0.65.1` | 6497 | 7.53 | 13.42 | 9.82
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 4936 | 12.99 | 13.12 | 12.99
| [tornado](https://pypi.org/project/tornado/) `6.1` | 2953 | 21.63 | 21.74 | 21.67
| [quart](https://pypi.org/project/quart/) `0.15.1` | 2213 | 28.42 | 28.91 | 28.90
| [django](https://pypi.org/project/django/) `3.2.3` | 1555 | 41.68 | 45.21 | 41.12

</details>

<h3 id="composite"> Composite stats </h3>
<details open>
<summary> Combined benchmarks results</summary>

Sorted by completed requests

| Framework | Requests completed | Avg Latency 50% (ms) | Avg Latency 75% (ms) | Avg Latency (ms) |
| --------- | -----------------: | -------------------: | -------------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.6` | 553635 | 5.06 | 8.51 | 6.41
| [muffin](https://pypi.org/project/muffin/) `0.78.0` | 490470 | 5.98 | 10.26 | 7.66
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 450315 | 7.19 | 11.97 | 9.04
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 373035 | 13.14 | 16.1 | 12.21
| [emmett](https://pypi.org/project/emmett/) `2.2.1` | 346905 | 15.73 | 20.84 | 17.7
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 322590 | 7.55 | 13.08 | 9.81
| [fastapi](https://pypi.org/project/fastapi/) `0.65.1` | 280140 | 11.33 | 19.98 | 14.63
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 226770 | 15.87 | 15.96 | 15.87
| [tornado](https://pypi.org/project/tornado/) `6.1` | 130230 | 22.66 | 22.78 | 22.69
| [quart](https://pypi.org/project/quart/) `0.15.1` | 115125 | 26.74 | 27.31 | 26.79
| [django](https://pypi.org/project/django/) `3.2.3` | 66840 | 45.93 | 50.83 | 45.85

</details>

## Conclusion

Nothing here, just some measures for you.

## License

Licensed under a MIT license (See LICENSE file)