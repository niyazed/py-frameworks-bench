# Async Python Web Frameworks comparison

https://klen.github.io/py-frameworks-bench/
----------
#### Updated: 2021-06-22

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
* [The Results](#the-results-2021-06-22)
    * [Accept a request and return HTML response with a custom dynamic header](#html)
    * [Parse path params, query string, JSON body and return a json response](#api)
    * [Parse uploaded file, store it on disk and return a text response](#upload)
    * [Composite stats ](#composite)



<img src='https://quickchart.io/chart?width=800&height=400&c=%7Btype%3A%22bar%22%2Cdata%3A%7Blabels%3A%5B%22blacksheep%22%2C%22muffin%22%2C%22falcon%22%2C%22starlette%22%2C%22emmett%22%2C%22sanic%22%2C%22fastapi%22%2C%22aiohttp%22%2C%22tornado%22%2C%22quart%22%2C%22django%22%5D%2Cdatasets%3A%5B%7Blabel%3A%22num%20of%20req%22%2Cdata%3A%5B467295%2C423960%2C393750%2C324615%2C292305%2C280365%2C242865%2C196455%2C105060%2C90660%2C59565%5D%7D%5D%7D%7D' />

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

2. "API" test: Check headers, parse path params, query string, JSON body and return a json
   response. The test simulates an JSON REST API.

3. "Upload" test: accept an uploaded file and store it on disk. The test
   simulates multipart formdata processing and work with files.


## The Results (2021-06-22)

<h3 id="html"> Accept a request and return HTML response with a custom dynamic header</h3>
<details open>
<summary> The test simulates just a single HTML response. </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.8` | 17127 | 3.23 | 4.79 | 3.69
| [muffin](https://pypi.org/project/muffin/) `0.80.0` | 15098 | 3.49 | 5.52 | 4.20
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 14061 | 3.67 | 5.95 | 4.51
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 12342 | 4.17 | 6.82 | 5.15
| [emmett](https://pypi.org/project/emmett/) `2.2.3` | 12232 | 4.47 | 6.79 | 5.19
| [fastapi](https://pypi.org/project/fastapi/) `0.65.2` | 8726 | 5.82 | 9.76 | 7.29
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 8171 | 6.36 | 10.22 | 7.84
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 6886 | 9.30 | 9.50 | 9.29
| [quart](https://pypi.org/project/quart/) `0.15.1` | 2769 | 23.02 | 24.88 | 23.12
| [tornado](https://pypi.org/project/tornado/) `6.1` | 2727 | 22.78 | 24.00 | 23.47
| [django](https://pypi.org/project/django/) `3.2.4` | 1660 | 38.65 | 42.10 | 38.57


</details>

<h3 id="api"> Parse path params, query string, JSON body and return a json response</h3>
<details open>
<summary> The test simulates a simple JSON REST API endpoint.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [muffin](https://pypi.org/project/muffin/) `0.80.0` | 9280 | 5.43 | 9.27 | 6.86
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.8` | 9094 | 5.84 | 9.18 | 7.01
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 9002 | 5.58 | 9.57 | 7.07
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 7151 | 7.04 | 12.09 | 8.91
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 6871 | 7.23 | 12.37 | 9.31
| [emmett](https://pypi.org/project/emmett/) `2.2.3` | 6024 | 8.73 | 13.73 | 10.71
| [fastapi](https://pypi.org/project/fastapi/) `0.65.2` | 5550 | 9.35 | 15.57 | 11.49
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 4162 | 15.34 | 15.61 | 15.38
| [tornado](https://pypi.org/project/tornado/) `6.1` | 2445 | 26.11 | 26.54 | 26.16
| [quart](https://pypi.org/project/quart/) `0.15.1` | 1792 | 34.83 | 36.78 | 35.70
| [django](https://pypi.org/project/django/) `3.2.4` | 1407 | 46.41 | 50.06 | 45.43

</details>

<h3 id="upload"> Parse uploaded file, store it on disk and return a text response</h3>
<details open>
<summary> The test simulates multipart formdata processing and work with files.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.8` | 4932 | 10.30 | 17.39 | 12.95
| [muffin](https://pypi.org/project/muffin/) `0.80.0` | 3886 | 12.83 | 22.26 | 16.45
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 3649 | 14.16 | 23.34 | 17.50
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 3187 | 16.55 | 26.36 | 20.14
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 2148 | 25.47 | 38.99 | 29.75
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 2049 | 31.18 | 31.73 | 31.21
| [fastapi](https://pypi.org/project/fastapi/) `0.65.2` | 1915 | 38.09 | 43.03 | 33.37
| [tornado](https://pypi.org/project/tornado/) `6.1` | 1832 | 34.80 | 35.28 | 34.94
| [quart](https://pypi.org/project/quart/) `0.15.1` | 1483 | 42.47 | 44.78 | 43.12
| [emmett](https://pypi.org/project/emmett/) `2.2.3` | 1231 | 49.02 | 57.71 | 51.92
| [django](https://pypi.org/project/django/) `3.2.4` | 904 | 69.48 | 78.94 | 70.67


</details>

<h3 id="composite"> Composite stats </h3>
<details open>
<summary> Combined benchmarks results</summary>

Sorted by completed requests

| Framework | Requests completed | Avg Latency 50% (ms) | Avg Latency 75% (ms) | Avg Latency (ms) |
| --------- | -----------------: | -------------------: | -------------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.8` | 467295 | 6.46 | 10.45 | 7.88
| [muffin](https://pypi.org/project/muffin/) `0.80.0` | 423960 | 7.25 | 12.35 | 9.17
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 393750 | 8.6 | 13.96 | 10.57
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 324615 | 12.23 | 19.3 | 14.6
| [emmett](https://pypi.org/project/emmett/) `2.2.3` | 292305 | 20.74 | 26.08 | 22.61
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 280365 | 9.25 | 15.31 | 11.55
| [fastapi](https://pypi.org/project/fastapi/) `0.65.2` | 242865 | 17.75 | 22.79 | 17.38
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 196455 | 18.61 | 18.95 | 18.63
| [tornado](https://pypi.org/project/tornado/) `6.1` | 105060 | 27.9 | 28.61 | 28.19
| [quart](https://pypi.org/project/quart/) `0.15.1` | 90660 | 33.44 | 35.48 | 33.98
| [django](https://pypi.org/project/django/) `3.2.4` | 59565 | 51.51 | 57.03 | 51.56

</details>

## Conclusion

Nothing here, just some measures for you.

## License

Licensed under a MIT license (See LICENSE file)