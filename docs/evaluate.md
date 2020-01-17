# Get started

You can quickly deploy DevOps images of Ping Identity solutions. These images are preconfigured to provide working instances of our solutions, either as single containers or an orchestrated set of containers. We use Docker to deploy the DevOps images in stable, network-enabled containers. For lightweight orchestration purposes, we use Docker Compose. For enterprise-level orchestration of containers, we use Kubernetes.

You'll need an evaluation license to use the DevOps resources. You'll clone our getting started repository, set up your DevOps environment, and deploy our full stack of solutions for DevOps using Docker Compose. When you first start the Docker stack, our full set of DevOps images is automatically pulled from our repository, if you haven't already pulled the images from [Docker Hub](https://hub.docker.com/u/pingidentity/). You can then choose to try out any one or more of the solutions, all preconfigured to interoperate.

## Prerequisites

  * Either [Docker CE for Windows](https://docs.docker.com/v17.12/install/) or [Docker for macOS](https://docs.docker.com/v17.12/docker-for-mac/install/).
  * [Git](https://git-scm.com/downloads).

What you'll need to do:

  1. Create a Ping Identity account, or sign on to your existing account and get a DevOps [evaluation license](#step2).
  2. [Save your DevOps credentials in a local text file](#step3).
  3. [Make a local copy of the DevOps directory](#step4), `${HOME}/projects/devops`.
  4. [Clone the DevOps repository](#step5), `https://github.com/pingidentity/pingidentity-devops-getting-started.git` to your local `${HOME}/projects/devops` directory.
  5. [Run our `setup` script](#step6) in `${HOME}/projects/devops/pingidentity-devops-getting-started` to quickly set up the DevOps environment.
  6. [Refresh your OS shell](#step7).
  7. [Use Docker Compose to deploy the full stack](#step8). This will run our [YAML configuration file](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/11-docker-compose/03-full-stack/docker-compose.yaml).
  8. [Log in to the management consoles for the solutions](#step9).
  9. [Persist your configuration changes](#step10).
  10. [Stop or bring down the stack](#step11).

  See **Procedures** for complete instructions.

You can then choose to:

  * Rerun the full stack evaluation.
  * [Deploy a solution as a standalone Docker container, or deploy a set of solutions using orchestration](deploy.md).
  * [Manage container and stack configurations](configDeploy.md).
  * [Customize the DevOps images](customImages.md).

## Procedures

  1.<a id="step1"/> [Create a Ping Identity account, or sign on to your existing account](https://www.pingidentity.com/en/account/sign-on.html).
  2. <a id="step2"/>You'll need a DevOps user name and DevOps key. Your DevOps user name is the email address associated with your Ping Identity account. Request your DevOps key using this [form](https://docs.google.com/forms/d/e/1FAIpQLSdgEFvqQQNwlsxlT6SaraeDMBoKFjkJVCyMvGPVPKcrzT3yHA/viewform).

      Your DevOps user name and key will be sent to your email. This will generally take only a few business hours.

  3.<a id="step3"/> Save your DevOps user name and key in a text file. It'll look something like this:

     ```text
     PING_IDENTITY_DEVOPS_USER=jsmith@example.com
     PING_IDENTITY_DEVOPS_KEY=e9bd26ac-17e9-4133-a981-d7a7509314b2
     ```

     > Be sure to use the exact variable names.

  4.<a id="step4"/> Make a local copy of the DevOps repository in this location: `${HOME}/projects/devops`.
  For example, enter:

      ```text
      mkdir -p ${HOME}/projects/devops
      cd ${HOME}/projects/devops
      ```
    > A common location will make it easier for us to help you if issues occur.

  5.<a name="step5"/> Clone the DevOps repository to the `${HOME}/projects/devops` directory on your local machine:

       `git clone https://github.com/pingidentity/pingidentity-devops-getting-started.git`

  6.<a id="step6"/> Go to the `${HOME}/projects/devops/pingidentity-devops-getting-started` directory and run our `setup` script to quickly and easily set up your local DevOps environment for the Ping Identity solutions. For example, enter:

     ```text
     cd pingidentity-devops-getting-started
     ./setup
     ```
     > The setup script also adds command aliases to make running Docker and Kubernetes commands easier.

  7.<a id="step7"/> Refresh your OS shell to make the command aliases available. For example, enter:

     ```text
     source ~/.bash_profile
     ```
     After refreshing your OS shell, enter `dhelp` to see the listing of the command aliases.

    > If the `dhelp` command isn't working, see [Troubleshooting](docs/troubleshooting/BASIC_TROUBLESHOOTING.md)

  8.<a id="step8"/> Deploy the full stack of solutions:

       a. To start the stack, on your local machine, go to the `pingidentity-devops-getting-started/11-docker-compose/03-full-stack` directory and enter:

        `docker-compose up -d`

          The full set of DevOps images is automatically pulled from our repository, if you haven't already pulled the images from [Docker Hub](https://hub.docker.com/u/pingidentity/).

       b. Use this command to display the logs as the stack starts:

        `docker-compose logs -f`

        Enter `Ctrl+C` to exit the display.

       c. Use either of these commands to display the status of the Docker containers in the stack:

        * `docker ps` (enter this at intervals)
        * `watch "docker container ls --format 'table {{.Names}}\t{{.Status}}'"`

       See the [Docker Compose overview](../11-docker-compose/README.md) for help with starting, stopping, and cleaning up our Docker stacks. You can also refer to the Docker Compose documentation [on the Docker site](https://docs.docker.com/compose/).

  9.<a id="step9"/> Log in to the management console for each of the solutions you want to use:

  * PingDataConsole for PingDirectory
        Console URL: https://localhost:8443/console
        Server: pingdirectory
        User: Administrator
        Password: 2FederateM0re

  * PingFederate
        Console URL: https://localhost:9999/pingfederate/app
        User: Administrator
        Password: 2FederateM0re

  * PingAccess
        Console URL: https://localhost:9000
        User: Administrator
        Password: 2FederateM0re

  * PingDataConsole for DataGovernance
        Console URL: https://localhost:8443/console
        Server: pingdatagovernance
        User: Administrator
        Password: 2FederateM0re

  * Apache Directory Studio for PingDirectory
        LDAP Port: 1389
        LDAP BaseDN: dc=example,dc=com
        Root Username: cn=administrator
        Root Password: 2FederateM0re

  10. (Recommended) Set up a local Docker volume to persist state and data for the stack whenever you bring the stack down. This will enable you to save any configuration changes you make to the product instances running in the stack. If you don't do this, the next time you bring up the stack, you'll need to repeat any configuration changes you might have made.

      You'll need to bind a Docker volume location to the Docker `/opt/out` directory for the container. Docker uses the `/opt/out` directory to store application data. To do this, for each container in the stack:

      a. Add a `volumes` section under the container entry in the `docker-compose.yaml` file you're using for the stack.
      b. Under the `volumes` section, add a location to persist your data. For the example:

         ```yaml
         pingfederate:
         .
         .
         .
         volumes:
          - /tmp/compose/pingfederate_1:/opt/out
         ```

         When the container starts, this will bind mount `/tmp/compose/pingfederate_1` to the `/opt/out` directory in the container. You're also able to view the product logs and data in the `/tmp/compose/pingfederate_1` directory.

      c. Repeat this process for the remaining container entries in the stack.

  11.<a id="step11"/> When you no longer want to run this full stack evaluation, you can either stop the running stack, or bring the stack down.

      Entering:

       `docker-compose stop`

      will stop the running stack without removing any of the containers or associated Docker networks.

      Entering:

       `docker-compose down`

       will remove all of the containers and associated Docker networks.

You can now choose to:

  * Rerun the full stack evaluation (`docker-compose up -d`).
  * [Deploy a solution as a standalone Docker container, or deploy a set of solutions using orchestration](deploy.md).
  * [Manage container and stack configurations](configDeploy.md).
  * [Customize the DevOps images](customImages.md).