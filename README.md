# Jenkins + Docker = Awesome CI

See the accompanying blog post [here](http://www.theguild.nl/jenkins-docker-awesome-ci/) for more information.

## How does it work?

The [run-containerized](run-containerized) script uses the `dockerfiles/ci/Dockerfile` in your project to build a container to run your tests in during the Jenkins job execution. Docker automatically caches containers so there's almost no overhead after the initial run of the job where the container is actually build.

The jobs workspace is mounted in the container under `/workspace`. Each container get's a `/cache` directory mounted that is dedicated to that job. Whatever is stored in this directory is persisted accross builds. This can be used for example to store Ruby Bundler gems, the local Maven repository etc.

For more information read the [run-containerized](run-containerized) script, it's well documented.

## Usage:

1. Make sure your Jenkins slaves can run Docker containers.
2. Make sure the user running your Jenkins jobs (usually `jenkins`) is allowed to run Docker containers.
3. Put the [run-containerized](run-containerized) script in your Jenkins `$PATH`.
4. Check in a Dockerfile into your project at `dockerfiles/ci/Dockerfile`.
5. Configure your jobs with an `Execute shell` step and prefix your CI command(s) with `run-containerized`, for example `run-containerized 'bin/ci'`.

## Tips 'n tricks

* Make sure that you configure your Dockerfile to store cachable artifacts in the `/cache` directory. For Ruby projects using Bundler this can be done by putting `RUN bundle config --global path /cache/` in the Dockerfile.
* If you need any services running during the build (for example PostgreSQL, Redis etc) make sure to include them in the Dockerfile. The file `/usr/local/bin/init-container` is automatically invoked just before your CI command is run, this can be used to start your services.

## Examples

See [example-dockerfiles](example-dockerfiles/) for some example Dockerfiles to use in your project. These files demonstrate the usage of the `/cache` directory and the starting of services using `/usr/local/bin/init-container`.

## Caveats

* Builds currently run as root inside the container. This is not as scary as it sounds but you should be aware of possible security risks when you allow untrusted people to run builds on your CI server (for example Pull Requests on a public project).
