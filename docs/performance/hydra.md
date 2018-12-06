---
id: performance-hydra
title: ORY Hydra
---

In this document you will find benchmark results for different endpoints of ORY Hydra. All benchmarks are executed
using [rakyll/hey](https://github.com/rakyll/hey). Please note that these benchmarks run against the in-memory storage
adapter of ORY Hydra. These benchmarks represent what performance you would get with a zero-overhead database implementation.

We do not include benchmarks against databases (e.g. MySQL or PostgreSQL) as the performance greatly differs between
deployments (e.g. request latency, database configuration) and tweaking individual things may greatly improve performance.
We believe, for that reason, that benchmark results for these database adapters are difficult to generalize and potentially
deceiving. They are thus not included.

This file is updated on every push to master. It thus represents the benchmark data for the latest version.

All benchmarks run 10.000 requests in total, with 100 concurrent requests. All benchmarks run on Circle-CI with a
["2 CPU cores and 4GB RAM"](https://support.circleci.com/hc/en-us/articles/360000489307-Why-do-my-tests-take-longer-to-run-on-CircleCI-than-locally-)
configuration.

## BCrypt

ORY Hydra uses BCrypt to obfuscate secrets of OAuth 2.0 Clients. When using flows such as the OAuth 2.0 Client Credentials
Grant, ORY Hydra validates the client credentials using BCrypt which causes (by design) CPU load. CPU load and performance
depend on the BCrypt cost which can be set using the environment variable `BCRYPT_COST`. For these benchmarks,
we have set `BCRYPT_COST=8`.

## OAuth 2.0

This section contains various benchmarks against OAuth 2.0 endpoints

### Token Introspection

```

Summary:
  Total:	0.3902 secs
  Slowest:	0.0289 secs
  Fastest:	0.0001 secs
  Average:	0.0036 secs
  Requests/sec:	25624.9747
  
  Total data:	1550000 bytes
  Size/request:	155 bytes

Response time histogram:
  0.000 [1]	|
  0.003 [4705]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.006 [2290]	|■■■■■■■■■■■■■■■■■■■
  0.009 [2312]	|■■■■■■■■■■■■■■■■■■■■
  0.012 [568]	|■■■■■
  0.014 [50]	|
  0.017 [30]	|
  0.020 [14]	|
  0.023 [19]	|
  0.026 [10]	|
  0.029 [1]	|


Latency distribution:
  10% in 0.0001 secs
  25% in 0.0002 secs
  50% in 0.0033 secs
  75% in 0.0064 secs
  90% in 0.0081 secs
  95% in 0.0092 secs
  99% in 0.0123 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0000 secs, 0.0001 secs, 0.0289 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0060 secs
  req write:	0.0000 secs, 0.0000 secs, 0.0076 secs
  resp wait:	0.0035 secs, 0.0001 secs, 0.0184 secs
  resp read:	0.0000 secs, 0.0000 secs, 0.0049 secs

Status code distribution:
  [200]	10000 responses



```

### Client Credentials Grant

This endpoint uses [BCrypt](#bcrypt).

```

Summary:
  Total:	18.4411 secs
  Slowest:	0.7037 secs
  Fastest:	0.0165 secs
  Average:	0.1786 secs
  Requests/sec:	542.2664
  
  Total data:	1570000 bytes
  Size/request:	157 bytes

Response time histogram:
  0.016 [1]	|
  0.085 [1007]	|■■■■■■■■■■■■
  0.154 [3395]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.223 [3325]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.291 [950]	|■■■■■■■■■■■
  0.360 [734]	|■■■■■■■■■
  0.429 [388]	|■■■■■
  0.498 [88]	|■
  0.566 [64]	|■
  0.635 [44]	|■
  0.704 [4]	|


Latency distribution:
  10% in 0.0850 secs
  25% in 0.1039 secs
  50% in 0.1822 secs
  75% in 0.2149 secs
  90% in 0.3024 secs
  95% in 0.3818 secs
  99% in 0.5025 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0001 secs, 0.0165 secs, 0.7037 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0228 secs
  req write:	0.0002 secs, 0.0000 secs, 0.1001 secs
  resp wait:	0.1777 secs, 0.0165 secs, 0.6880 secs
  resp read:	0.0005 secs, 0.0000 secs, 0.0814 secs

Status code distribution:
  [200]	10000 responses



```

## OAuth 2.0 Client Management

### Creating OAuth 2.0 Clients

This endpoint uses [BCrypt](#bcrypt) and generates IDs and secrets by reading from  which negatively impacts
performance. Performance will be better if IDs and secrets are set in the request as opposed to generated by ORY Hydra.

```
This test is currently disabled due to issues with /dev/urandom being inaccessible in the CI.
```

### Listing OAuth 2.0 Clients

```

Summary:
  Total:	0.2943 secs
  Slowest:	0.0199 secs
  Fastest:	0.0001 secs
  Average:	0.0027 secs
  Requests/sec:	33982.7469
  
  Total data:	4370000 bytes
  Size/request:	437 bytes

Response time histogram:
  0.000 [1]	|
  0.002 [5860]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.004 [1247]	|■■■■■■■■■
  0.006 [1168]	|■■■■■■■■
  0.008 [723]	|■■■■■
  0.010 [564]	|■■■■
  0.012 [237]	|■■
  0.014 [111]	|■
  0.016 [65]	|
  0.018 [17]	|
  0.020 [7]	|


Latency distribution:
  10% in 0.0001 secs
  25% in 0.0001 secs
  50% in 0.0007 secs
  75% in 0.0046 secs
  90% in 0.0080 secs
  95% in 0.0096 secs
  99% in 0.0136 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0000 secs, 0.0001 secs, 0.0199 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0072 secs
  req write:	0.0000 secs, 0.0000 secs, 0.0065 secs
  resp wait:	0.0025 secs, 0.0000 secs, 0.0199 secs
  resp read:	0.0001 secs, 0.0000 secs, 0.0079 secs

Status code distribution:
  [200]	10000 responses



```

### Fetching a specific OAuth 2.0 Client

```

Summary:
  Total:	0.2901 secs
  Slowest:	0.0207 secs
  Fastest:	0.0001 secs
  Average:	0.0027 secs
  Requests/sec:	34475.6043
  
  Total data:	4350000 bytes
  Size/request:	435 bytes

Response time histogram:
  0.000 [1]	|
  0.002 [5860]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.004 [1380]	|■■■■■■■■■
  0.006 [1186]	|■■■■■■■■
  0.008 [734]	|■■■■■
  0.010 [444]	|■■■
  0.012 [248]	|■■
  0.014 [108]	|■
  0.017 [26]	|
  0.019 [6]	|
  0.021 [7]	|


Latency distribution:
  10% in 0.0001 secs
  25% in 0.0001 secs
  50% in 0.0008 secs
  75% in 0.0045 secs
  90% in 0.0078 secs
  95% in 0.0097 secs
  99% in 0.0129 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0000 secs, 0.0001 secs, 0.0207 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0048 secs
  req write:	0.0000 secs, 0.0000 secs, 0.0054 secs
  resp wait:	0.0025 secs, 0.0000 secs, 0.0205 secs
  resp read:	0.0001 secs, 0.0000 secs, 0.0055 secs

Status code distribution:
  [200]	10000 responses



```