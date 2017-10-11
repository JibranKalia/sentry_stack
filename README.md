# Sentry Quick Testing Swarm Stack

This docker service stack describes a simple Sentry with Zenko S3 setup for quick testing
with non-production data.

## Preparing

Swarm mode needs to be enabled on the local docker daemon. See
[this tutorial](https://docs.docker.com/engine/swarm/swarm-tutorial/)
for more information on Swarm mode.

## Deploying

Deploy the stack:

```
$ docker stack deploy -c docker-stack.yml sentry
ID                  NAME                MODE                REPLICAS            IMAGE                                PORTS
926t3wn0ly1g        sentry_s3           replicated          1/1                 tomasbisi/s3server:supervisorcache   *:0->8100/tcp,*:8000->8000/tcp
aziwhvicugnd        sentry_frontend     replicated          1/1                 tomasbisi/sentry_frontend:latest     *:5000->5000/tcp
i5k4jussjo7h        sentry_cache        replicated          1/1                 redis:alpine                         *:0->6379/tcp
nx6dkyfjnl5s        sentry_backend      replicated          1/1                 tomasbisi/sentry_backend:latest      *:8200->8200/tcp
ojwrc3jkmg2a        sentry_dynamodb     replicated          1/1                 jibrankalia/dynamodb:latest          *:0->8300/tcp
```

Check that the services are up:

```
$ docker stack services sentry
ID                  NAME                MODE                REPLICAS            IMAGE                                PORTS
926t3wn0ly1g        sentry_s3           replicated          1/1                 tomasbisi/s3server:supervisorcache   *:0->8100/tcp,*:8000->8000/tcp
aziwhvicugnd        sentry_frontend     replicated          1/1                 tomasbisi/sentry_frontend:latest     *:5000->5000/tcp
i5k4jussjo7h        sentry_cache        replicated          1/1                 redis:alpine                         *:0->6379/tcp
nx6dkyfjnl5s        sentry_backend      replicated          1/1                 tomasbisi/sentry_backend:latest      *:8200->8200/tcp
ojwrc3jkmg2a        sentry_dynamodb     replicated          1/1                 jibrankalia/dynamodb:latest          *:0->8300/tcp
```

## Testing

Using [awscli](https://aws.amazon.com/cli/), we can perform S3 operations
on our Zenko stack:
 > IMPORTANT: when using default port 80, it should never be specified after the
 > endpoint address. If using a custom port, it must be specified.
```
$ export AWS_ACCESS_KEY_ID=accessKey1
$ export AWS_SECRET_ACCESS_KEY=verySecretKey1
$ aws s3 --endpoint http://localhost mb s3://bucket1 --region=us-east-1
make_bucket: bucket1
$ aws s3 --endpoint http://localhost ls
2017-06-15 16:42:58 bucket1
$ aws s3 --endpoint http://localhost cp README.md s3://bucket1
upload: ./README.md to s3://bucket1/README.md
$ aws s3 --endpoint http://localhost ls s3://bucket1
2017-06-15 17:36:10       1510 README.md
```
