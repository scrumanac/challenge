# Site Reliability Engineer Challenge

## Introduction

At Scrumanac we are scaling rapidly and we need SREs to help us prevent downtime and provide visibility for our engineers into what things may go wrong.

## Requirement

1. We value a **clean**, **simple** working solution.
2. The application must be run in Docker, candidate must provide `docker-compose.yml` and `start.sh` bash script at the root of the project, which should setup all relevant dependencies.
3. The solution must be written in Golang.
4. Candidate must submit the project as a private git repository (github.com / bitbucket.com / gitlab.com). Repository must grant read access with the public SSH [key](id_rsa.pub).
5. Having unit/integration tests is a strong bonus.
6. The solution must be production ready with appropriate documentation.

## Problem Statement

You will build a configurable http ingress proxy with built in observability that can be used as a kubernetes sidecar.

Feature requirements:
  - Support prometheus metrics (design appropriate metrics to be exposed).
  - Support retry for failed requests (5xx status code).
  - Support timeout for requests.
  - Use [konfig](https://github.com/lalamove/konfig) to parse yaml config file.

Bonus features:
  - Circuit breaker implementation for request failures.
  - Rate limiting.
  
Example yaml config file that will be used as an input:
```yaml
port: 80

metrics:
  path: "/metrics"
  port: 9100

paths: 
- path: "/foo*"
  method: "GET"
  forwardPort: 8080
  # max number of times the proxy can retry a failed request (5xx status code)
  maxRetry: 3
  timeout: 1s
  circuitBreaking:
    # number of consecutive errors after which the circuit breaker should reject incoming requests 
    maxError: 3
    # time for which incoming requests should be rejected
    rejectionPeriod: 5s
  rateLimiting:
    # maximum number of requests to be allowed in the given time interval
    maxAmount: 5000
    expiration: 5s
```
 
 Questions? We love to answer: hr.factoryirisii+techchallenge@gmail.com
