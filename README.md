# gitserver-http [![Build Status](https://travis-ci.org/cirocosta/gitserver-http.svg?branch=master)](https://travis-ci.org/cirocosta/gitserver-http)

> A git server with Nginx as the HTTP frontend and fast cgi wrapper for running the git http backend

## Deployment

This is currently manually deployed as kim-nginx on ECS.  You need to log into AWS, and then perform then execute the following with AWS CLI v2. The full instructions can be found on AWS Web Console by going to ECR, clicking "kim-nginx" then "View Push Commands" in the top right corner.

```
docker build -t kim-nginx .
docker tag kim-nginx:latest xxxxxx.dkr.ecr.eu-central-1.amazonaws.com/kim-nginx:latest
docker push xxxxxx.dkr.ecr.eu-central-1.amazonaws.com/kim-nginx:latest
```

Then on AWS web console:

1. Go to Elastic Container Service
2. Click on "Task Definitions"
3. Click on "Nginx-Git-Server"
4. Click on the most recent revision
5. Click on "Update Definition" in top right corner
6. Click next until then end and then create with existing options.

After executing the above, the running tasks should update to the new latest revision after a few minutes.

## Force Reboot by "Re-deploying"

1. Go to Elastic Container Service
2. Under the "Amazon ECS" sidebar header, click on
"Clusters" (there are two mentions of "Clusters" in the sidebar, click on the right one.)
3. Click on "Internal-Services"
4. Click on "Nginx-Git-Server" in the list
5. Click "Update" in top right corner
6. Check the box "force-redeploy"
7. Click "Skip to Review"
8. Click "Update Service"


## Local Development

In order to test serving repositories from ./example/initial, do the following:
1. Build an image based on your Nginx config.

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

Note: If you run into issues with exit code 128, build your docker image with `--no-cache` option.

# Local Testing

Run a local test with `make test`. Note, you have to build the image with `--no-cache` after each test run or you run into `exited with code 128`.
