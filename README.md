# VPN Network Benchmark

[![Coverage Status](https://codecov.io/gh/mozilla-services/vpn-network-benchmark/branch/main/graph/badge.svg?token=JW9B9YTOE0)](https://codecov.io/gh/mozilla-services/vpn-network-benchmark)
[![Security audit](https://github.com/mozilla-services/vpn-network-benchmark/actions/workflows/scheduled-audit.yml/badge.svg)](https://github.com/mozilla-services/vpn-network-benchmark/actions/workflows/scheduled-audit.yml)

Microservice for the Mozilla VPN in-app network upload benchmark.

## Endpoints

`/upload`:
- POST only: Returns `200` data is readable

`/health`:
- GET: Returns `200`
- POST: Returns `200`

## Development pre-requisites

### Rust

https://www.rust-lang.org/tools/install

## Run server

1. Build the project
`cargo build`

2. Run the server
`cargo run`

## Testing

### Run tests

Run the integration tests:
`cargo test`

### Test upload endpoint

For testing purposes you could create a file with random data using for example [dd](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/dd.html) from the terminal. The following command creates 1 MB of data:
```
dd if=/dev/random of=random_data.bin bs=1M count=1
```

You can then POST data to the endpoint `/upload` with:
```
curl -i -X POST --data-binary @random_data.bin -H "Content-type: application/json" http://localhost:{PORT}/upload
````

## Deployment

This service is deployed using docker containers.

1. [Install docker](https://docs.docker.com/engine/install/)

2. Build an image with:
`docker build -t vpn-network-benchmark:latest .`

3. To run, set environment variables and forward ports:
`docker run -e HOST=0.0.0.0 -e PORT=8080 -p 8080:8080 vpn-network-benchmark:latest`
