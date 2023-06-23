# Async Python Web Frameworks comparison



https://klen.github.io/py-frameworks-bench/
----------
#### Updated: 2022-03-14

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
* [The Results](#the-results-2022-03-14)
    * [Accept a request and return HTML response with a custom dynamic header](#html)
    * [Parse path params, query string, JSON body and return a json response](#api)
    * [Parse uploaded file, store it on disk and return a text response](#upload)
    * [Composite stats ](#composite)



<img src='https://quickchart.io/chart?width=800&height=400&c=%7Btype%3A%22bar%22%2Cdata%3A%7Blabels%3A%5B%22blacksheep%22%2C%22sanic%22%2C%22muffin%22%2C%22falcon%22%2C%22starlette%22%2C%22baize%22%2C%22emmett%22%2C%22fastapi%22%2C%22aiohttp%22%2C%22tornado%22%2C%22quart%22%2C%22django%22%5D%2Cdatasets%3A%5B%7Blabel%3A%22num%20of%20req%22%2Cdata%3A%5B519825%2C470400%2C469725%2C436800%2C365490%2C349425%2C328275%2C255615%2C209310%2C121185%2C109755%2C38610%5D%7D%5D%7D%7D' />

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


## The Results (2022-03-14)

<h3 id="html"> Accept a request and return HTML response with a custom dynamic header</h3>
<details open>
<summary> The test simulates just a single HTML response. </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.2.5` | 18546 | 2.80 | 4.53 | 3.41
| [muffin](https://pypi.org/project/muffin/) `0.87.0` | 16571 | 3.09 | 5.17 | 3.83
| [sanic](https://pypi.org/project/sanic/) `21.12.1` | 15558 | 4.70 | 5.14 | 4.08
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 15554 | 3.29 | 5.49 | 4.08
| [baize](https://pypi.org/project/baize/) `0.15.0` | 13880 | 3.69 | 6.21 | 4.58
| [starlette](https://pypi.org/project/starlette/) `0.17.1` | 13797 | 3.70 | 6.16 | 4.60
| [emmett](https://pypi.org/project/emmett/) `2.4.5` | 13380 | 5.54 | 6.10 | 4.75
| [fastapi](https://pypi.org/project/fastapi/) `0.75.0` | 9060 | 5.46 | 9.79 | 7.03
| [aiohttp](https://pypi.org/project/aiohttp/) `3.8.1` | 7240 | 8.74 | 9.01 | 8.84
| [quart](https://pypi.org/project/quart/) `0.16.3` | 3425 | 18.99 | 20.08 | 18.68
| [tornado](https://pypi.org/project/tornado/) `6.1` | 3232 | 19.76 | 19.94 | 19.81
| [django](https://pypi.org/project/django/) `4.0.3` | 1002 | 59.00 | 66.26 | 63.72


</details>

<h3 id="api"> Parse path params, query string, JSON body and return a json response</h3>
<details open>
<summary> The test simulates a simple JSON REST API endpoint.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [sanic](https://pypi.org/project/sanic/) `21.12.1` | 10777 | 6.97 | 7.67 | 5.90
| [blacksheep](https://pypi.org/project/blacksheep/) `1.2.5` | 10505 | 4.70 | 8.16 | 6.07
| [muffin](https://pypi.org/project/muffin/) `0.87.0` | 10319 | 4.79 | 8.41 | 6.17
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 10133 | 4.88 | 8.61 | 6.28
| [starlette](https://pypi.org/project/starlette/) `0.17.1` | 8135 | 6.03 | 10.76 | 7.83
| [emmett](https://pypi.org/project/emmett/) `2.4.5` | 7091 | 7.17 | 11.58 | 9.12
| [baize](https://pypi.org/project/baize/) `0.15.0` | 6581 | 9.96 | 10.24 | 9.71
| [fastapi](https://pypi.org/project/fastapi/) `0.75.0` | 5882 | 8.36 | 15.16 | 10.85
| [aiohttp](https://pypi.org/project/aiohttp/) `3.8.1` | 4496 | 14.15 | 14.32 | 14.24
| [tornado](https://pypi.org/project/tornado/) `6.1` | 2780 | 22.95 | 23.17 | 23.02
| [quart](https://pypi.org/project/quart/) `0.16.3` | 2146 | 29.42 | 30.05 | 29.81
| [django](https://pypi.org/project/django/) `4.0.3` | 883 | 68.00 | 71.74 | 72.37

</details>

<h3 id="upload"> Parse uploaded file, store it on disk and return a text response</h3>
<details open>
<summary> The test simulates multipart formdata processing and work with files.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.2.5` | 5604 | 8.87 | 15.77 | 11.40
| [sanic](https://pypi.org/project/sanic/) `21.12.1` | 5025 | 10.44 | 16.83 | 12.72
| [muffin](https://pypi.org/project/muffin/) `0.87.0` | 4425 | 11.14 | 19.99 | 14.43
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 3433 | 14.56 | 25.48 | 18.73
| [baize](https://pypi.org/project/baize/) `0.15.0` | 2834 | 21.89 | 24.48 | 22.57
| [starlette](https://pypi.org/project/starlette/) `0.17.1` | 2434 | 20.10 | 36.39 | 26.26
| [aiohttp](https://pypi.org/project/aiohttp/) `3.8.1` | 2218 | 28.81 | 29.09 | 28.84
| [fastapi](https://pypi.org/project/fastapi/) `0.75.0` | 2099 | 23.61 | 41.91 | 30.44
| [tornado](https://pypi.org/project/tornado/) `6.1` | 2067 | 30.89 | 31.09 | 30.95
| [quart](https://pypi.org/project/quart/) `0.16.3` | 1746 | 36.68 | 37.58 | 36.63
| [emmett](https://pypi.org/project/emmett/) `2.4.5` | 1414 | 41.83 | 50.86 | 45.21
| [django](https://pypi.org/project/django/) `4.0.3` | 689 | 86.45 | 89.44 | 92.51


</details>

<h3 id="composite"> Composite stats </h3>
<details open>
<summary> Combined benchmarks results</summary>

Sorted by completed requests

| Framework | Requests completed | Avg Latency 50% (ms) | Avg Latency 75% (ms) | Avg Latency (ms) |
| --------- | -----------------: | -------------------: | -------------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.2.5` | 519825 | 5.46 | 9.49 | 6.96
| [sanic](https://pypi.org/project/sanic/) `21.12.1` | 470400 | 7.37 | 9.88 | 7.57
| [muffin](https://pypi.org/project/muffin/) `0.87.0` | 469725 | 6.34 | 11.19 | 8.14
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 436800 | 7.58 | 13.19 | 9.7
| [starlette](https://pypi.org/project/starlette/) `0.17.1` | 365490 | 9.94 | 17.77 | 12.9
| [baize](https://pypi.org/project/baize/) `0.15.0` | 349425 | 11.85 | 13.64 | 12.29
| [emmett](https://pypi.org/project/emmett/) `2.4.5` | 328275 | 18.18 | 22.85 | 19.69
| [fastapi](https://pypi.org/project/fastapi/) `0.75.0` | 255615 | 12.48 | 22.29 | 16.11
| [aiohttp](https://pypi.org/project/aiohttp/) `3.8.1` | 209310 | 17.23 | 17.47 | 17.31
| [tornado](https://pypi.org/project/tornado/) `6.1` | 121185 | 24.53 | 24.73 | 24.59
| [quart](https://pypi.org/project/quart/) `0.16.3` | 109755 | 28.36 | 29.24 | 28.37
| [django](https://pypi.org/project/django/) `4.0.3` | 38610 | 71.15 | 75.81 | 76.2

</details>

## Conclusion

Nothing here, just some measures for you.

## License

Licensed under a MIT license (See LICENSE file)


## Other Resources
List of Py Frameworks:
- https://deepsource.com/blog/new-python-web-frameworks

In depth Experiment on Blacksheep:
- https://medium.com/@pond5-technology/a-new-microservice-using-a-lesser-known-framework-720f6e8b7cad#:~:text=BlackSheep%20demonstrates%20an%20ability%20to,being%20a%20factor%20of%20three.
