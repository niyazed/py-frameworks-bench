# Async Python Web Frameworks comparison

https://klen.github.io/py-frameworks-bench/
----------
#### Updated: 2021-06-07

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
* [The Results](#the-results-2021-06-07)
    * [Accept a request and return HTML response with a custom dynamic header](#html)
    * [Parse uploaded file, store it on disk and return a text response](#upload)
    * [Parse path params, query string, JSON body and return a json response](#api)
    * [Composite stats ](#composite)



<img src='https://quickchart.io/chart?width=800&height=400&c=%7Btype%3A%22bar%22%2Cdata%3A%7Blabels%3A%5B%22blacksheep%22%2C%22muffin%22%2C%22falcon%22%2C%22starlette%22%2C%22emmett%22%2C%22sanic%22%2C%22fastapi%22%2C%22aiohttp%22%2C%22tornado%22%2C%22quart%22%2C%22django%22%5D%2Cdatasets%3A%5B%7Blabel%3A%22num%20of%20req%22%2Cdata%3A%5B553080%2C488055%2C458100%2C375720%2C343395%2C321480%2C282780%2C226065%2C130800%2C112875%2C70065%5D%7D%5D%7D%7D' />

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


## The Results (2021-06-07)

<h3 id="html"> Accept a request and return HTML response with a custom dynamic header</h3>
<details open>
<summary> The test simulates just a single HTML response. </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.6` | 19619 | 2.71 | 4.19 | 3.23
| [muffin](https://pypi.org/project/muffin/) `0.79.1` | 17259 | 3.03 | 4.79 | 3.67
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 16225 | 3.16 | 5.17 | 3.91
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 14199 | 3.60 | 5.88 | 4.48
| [emmett](https://pypi.org/project/emmett/) `2.2.2` | 13994 | 3.66 | 6.02 | 4.54
| [fastapi](https://pypi.org/project/fastapi/) `0.65.1` | 10084 | 4.91 | 8.41 | 6.31
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 9210 | 5.38 | 9.12 | 6.97
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 7755 | 8.20 | 8.26 | 8.25
| [tornado](https://pypi.org/project/tornado/) `6.1` | 3510 | 18.14 | 18.33 | 18.23
| [quart](https://pypi.org/project/quart/) `0.15.1` | 3475 | 19.01 | 19.85 | 18.43
| [django](https://pypi.org/project/django/) `3.2.4` | 1969 | 31.32 | 35.95 | 32.52


</details>

<h3 id="upload"> Parse uploaded file, store it on disk and return a text response</h3>
<details open>
<summary> The test simulates multipart formdata processing and work with files.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.6` | 6224 | 7.84 | 14.03 | 10.26
| [muffin](https://pypi.org/project/muffin/) `0.79.1` | 4711 | 10.48 | 18.75 | 13.56
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 4430 | 11.03 | 19.65 | 14.53
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 3858 | 12.79 | 22.23 | 16.68
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 2570 | 19.09 | 34.01 | 24.87
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 2436 | 26.25 | 26.42 | 26.26
| [fastapi](https://pypi.org/project/fastapi/) `0.65.1` | 2329 | 20.88 | 37.19 | 27.44
| [tornado](https://pypi.org/project/tornado/) `6.1` | 2305 | 27.71 | 27.85 | 27.77
| [quart](https://pypi.org/project/quart/) `0.15.1` | 1870 | 34.15 | 34.91 | 34.20
| [emmett](https://pypi.org/project/emmett/) `2.2.2` | 1586 | 37.12 | 45.56 | 40.31
| [django](https://pypi.org/project/django/) `3.2.4` | 1048 | 60.45 | 67.23 | 60.96


</details>

<h3 id="api"> Parse path params, query string, JSON body and return a json response</h3>
<details open>
<summary> The test simulates a simple JSON REST API endpoint.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.6` | 11029 | 4.47 | 7.83 | 5.77
| [muffin](https://pypi.org/project/muffin/) `0.79.1` | 10567 | 4.67 | 8.08 | 6.02
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 10457 | 4.80 | 8.16 | 6.09
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 8279 | 5.93 | 10.36 | 7.70
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 7792 | 6.23 | 10.86 | 8.22
| [emmett](https://pypi.org/project/emmett/) `2.2.2` | 7313 | 6.61 | 11.63 | 8.88
| [fastapi](https://pypi.org/project/fastapi/) `0.65.1` | 6439 | 7.57 | 13.56 | 9.90
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 4880 | 13.04 | 13.27 | 13.11
| [tornado](https://pypi.org/project/tornado/) `6.1` | 2905 | 22.01 | 22.23 | 22.03
| [quart](https://pypi.org/project/quart/) `0.15.1` | 2180 | 28.83 | 29.63 | 29.32
| [django](https://pypi.org/project/django/) `3.2.4` | 1654 | 39.47 | 42.65 | 38.67

</details>

<h3 id="composite"> Composite stats </h3>
<details open>
<summary> Combined benchmarks results</summary>

Sorted by completed requests

| Framework | Requests completed | Avg Latency 50% (ms) | Avg Latency 75% (ms) | Avg Latency (ms) |
| --------- | -----------------: | -------------------: | -------------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.6` | 553080 | 5.01 | 8.68 | 6.42
| [muffin](https://pypi.org/project/muffin/) `0.79.1` | 488055 | 6.06 | 10.54 | 7.75
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 458100 | 6.92 | 11.85 | 8.89
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 375720 | 9.54 | 16.75 | 12.35
| [emmett](https://pypi.org/project/emmett/) `2.2.2` | 343395 | 15.8 | 21.07 | 17.91
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 321480 | 7.55 | 13.21 | 9.91
| [fastapi](https://pypi.org/project/fastapi/) `0.65.1` | 282780 | 11.12 | 19.72 | 14.55
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 226065 | 15.83 | 15.98 | 15.87
| [tornado](https://pypi.org/project/tornado/) `6.1` | 130800 | 22.62 | 22.8 | 22.68
| [quart](https://pypi.org/project/quart/) `0.15.1` | 112875 | 27.33 | 28.13 | 27.32
| [django](https://pypi.org/project/django/) `3.2.4` | 70065 | 43.75 | 48.61 | 44.05

</details>

## Conclusion

Nothing here, just some measures for you.

## License

Licensed under a MIT license (See LICENSE file)