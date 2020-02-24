# gitserver-http [![Build Status](https://travis-ci.org/cirocosta/gitserver-http.svg?branch=master)](https://travis-ci.org/cirocosta/gitserver-http)

> A git server with Nginx as the HTTP frontend and fast cgi wrapper for running the git http backend


## Usage

In order to test serving repositories from ./example/initial, do the following:
1. Build an image based on your nginx config.

```sh
docker build -t local-gitserver-http .
```

2. Run the image to serve the repos.
```sh
make example
```

Then you can either query your repos, like:
```sh
curl localhost:8080/myrepo.git/info/refs
```

Or actually clone a repo:
```sh
git clone http://localhost:8080/myrepo.git
```

While doing this, in the tab where you have `make example` executed, you'll be able to see whether your request hit the cache.
