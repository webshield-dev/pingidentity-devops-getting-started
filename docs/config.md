# Configure deployments

> If you remove any of the existing configurations for a Ping Identity solution, the solution may no longer interoperate with other solutions in the Docker stack.

You can configure deployments using:

  * [Server profiles](docs/profiles.md)

  The server profiles supply configuration, data, and environment information to the solution containers at startup. You can use our server profiles, or use them as a baseline for creating your own. You'll find these in [Server Profiles](docs/server-profiles.README.md).

  * [YAML files for orchestrated solutions](docs/yamlFiles.md)

  In the orchestration-related directories for the [deployment examples](../README.md), you'll find the YAML files used to configure the Docker stack deployment. The YAML files can contain startup configuration settings and/or references to startup configuration settings (such as, environment variables) for the stack. You can try different configuration settings using these YAML files, or use them as a baseline for creating your own.

  * [Environment variables](docs/environmentVars.md)

  We use environment variables for certain startup and runtime configuration settings of the Docker containers. There are environment variables that are common to all DevOps images. You'll find these in the [PingBase image directory](https://pingidentity-devops.gitbook.io/devops/docker-images/pingbase). There are also solution-specific environment variables. You'll find these in the [Docker image directory](https://pingidentity-devops.gitbook.io/devops/docker-images/pingbase) for each available solution. You can try different configuration settings using the common and solution-specific environment variables for our deployment examples, or use them in your own custom deployments.

  * [Shell scripts (hooks)](docs/hooks.md)

  Hooks are DevOps shell scripts, generally specific to a solution, that you can use to automate certain operations. You'll find the hooks for a solution in the [Docker builds solution directories](../../pingidentity-docker-builds).

  * [Release tags](docs/releaseTags.md)

  We use sets of tags for each released DevOps image. These tags identify whether the image is a specific stable release, the latest stable release, or current (potentially unstable) builds. You'll find the release tag information in [Docker images](docker-images/README.md). You can try different tags in either the standalone startup scripts for the deployment examples, or the YAML files for the orchestrated deployment examples.