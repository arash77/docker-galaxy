# Changelog

## 0.1: Initial release!
    - with Apache2, PostgreSQL and Tool Shed integration
## 0.2: complete new Galaxy stack.
   - with nginx, uwsgi, proftpd, docker, supervisord and SLURM
## 0.3: Add Interactive Environments
   - IPython in docker in Galaxy in docker
   - advanged logging
## 0.4:
   - base the image on toolshed/requirements with all required Galaxy dependencies
   - use Ansible roles to build large parts of the image
   - export the supervisord web interface on port 9002
   - enable Galaxy reports webapp
## 15.07:
  - `install-biojs` can install BioJS visualisations into Galaxy
  - `add-tool-shed` can be used to activate third party Tool Sheds in child Dockerfiles
  - many documentation improvements
  - RStudio is now part of Galaxy and this Image
  - configurable postgres UID/GID by @chambm
  - smarter starting of postgres during Tool installations by @shiltemann
## 15.10:
  - new Galaxy 15.10 release
  - fix https://github.com/bgruening/docker-galaxy-stable/issues/94
## 16.01:
  - enable Travis testing for all builds and PR
  - offer new [yaml based tool installations](https://github.com/galaxyproject/ansible-galaxy-tools/blob/master/files/tool_list.yaml.sample)
  - enable dynamic UWSGI processes and threads with `-e UWSGI_PROCESSES=2` and `-e UWSGI_THREADS=4`
  - enable dynamic Galaxy handlers `-e GALAXY_HANDLER_NUMPROCS=2`
  - Addition of a new `lite` mode contributed by @kellrott
  - first release with Jupyter integration
## 16.04:
  - include a Galaxy-bare mode, enable with `-e BARE=True`
  - first release with [HTCondor](https://research.cs.wisc.edu/htcondor/) installed and pre-configured
## 16.07:
  - documentation and tests updates for SLURM integration by @mvdbeek
  - first version with initial Docker compose support (proftpd ✔️)
  - SFTP support by @zfrenchee
## 16.10:
   - [HTTPS support](https://github.com/bgruening/docker-galaxy-stable/pull/240 ) by @zfrenchee and @mvdbeek
## 17.01:
  - enable Conda dependency resolution by default
  - [new Galaxy version](https://docs.galaxyproject.org/en/master/releases/17.01_announce.html)
  - more compose work (slurm, postgresql)
## 17.05:
   - add PROXY_PREFIX variable to enable automatic configuration of Galaxy running under some prefix (@abretaud)
   - enable quota by default (just the funtionality, not any specific value)
   - HT-Condor is now supported in compose with semi-autoscaling and BioContainers
   - Galaxy Docker Compose is completely under Travis testing and available with SLURM and HT-Condor
   - using Docker `build-arg`s for GALAXY_RELEASE and GALAXY_REPO
## 17.09:
   - much improved documentation about using Galaxy Docker and an external cluster (@rhpvorderman)
   - CVMFS support - mounting in 4TB of pre-build reference data (@chambm)
   - Singularity support and tests (compose only)
   - more work on K8s support and testing (@jmchilton)
   - using .env files to configure the compose setup for SLURM, Condor, K8s, SLURM-Singularity, Condor-Docker
## 18.01:
   - tracking the Galaxy release_18.01 branch
   - uwsgi work to adopt to changes for 18.01
   - remove nodejs-legacy & npm from Dockerfile and install latest version from ansible-extras
   - initial galaxy.ini → galaxy.yml integration
   - grafana and influxdb container (compose)
   - Galaxy telegraf integration to push to influxdb (compose)
   - added some documentation (compose)
## 18.05:
   - Nothing very special, but a awesome Galaxy release as usual
## 18.09:
   - new and more powerful orchestration build script (build-orchestration-images.sh) by @pcm32
   - a lot of bug-fixes to the compose setup by @abretaud
## 19.01:
   - This is featuring the latest and greatest from the Galaxy community
   - Please note that this release will be the last release which is based on `ubuntu:14.04` and PostgreSQL 9.3.
     We will migrate to `ubuntu:18.04` and a newer PostgreSQL version in `19.05`. Furthermore, we will not
     support old Galaxy tool dependencies.
## 19.05:
   - The image is now based on `ubuntu:18.04` (instead of ubuntu:14.04 previously) and PostgreSQL 11.5 (9.3 previously).
     See [migration documention](#Postgresql-migration) to migrate the postgresql database from 9.3 to 11.5.
   - We not longer support old Galaxy tool dependencies.
## 20.05:
   - Featuring Galaxy 20.05
   - Completely reworked compose setup
   - The default admin password and apikey (`GALAXY_DEFAULT_ADMIN_PASSWORD` and `GALAXY_DEFAULT_ADMIN_KEY`) have changed: the password is now `password` (instead of `admin`) and the apikey `fakekey` (instead of `admin`).
## 20.09:
   - Featuring Galaxy 20.09
## 24.1:
   - Deprecating the `compose` setup.
   - Complete new setup, adjusting to the latest Galaxy stack.
   - Base Ubuntu Image: Upgraded from version 18.04 to 22.04
   - Galaxy: Upgraded from version 20.09 to 24.1
   - PostgreSQL: Upgraded from version 11 to 15
   - Python3: Upgraded from version 3.7 to 3.10 (Python 3.10 is set as the default interpreter)
   - The dockerfile now uses a multi-stage build to reduce the final image size and include only necessary files.
   - New Service Support:
     - Gunicorn: Replaces uWSGI as the web server for Galaxy. Installed by default inside Galaxy's virtual environment. Configured Nginx to proxy Gunicorn enabled on port 4001.
     - Celery: Installed by default inside Galaxy's virtual environment. Enabled Celery for distributed task queues and Celery Beat for periodic task running. RabbitMQ serves as the broker for Celery (if RabbitMQ is disabled, it defaults to PostgreSQL database connection).
     - Redis is used as the backend for Celery (if Redis is disabled, it defaults to a SQLite database). Flower service is added for monitoring and debugging Celery.
     - RabbitMQ Management: Enabled the RabbitMQ management plugin on port 15672 for managing and monitoring the RabbitMQ server. The dashboard is exposed via Nginx and is accessible at the /rabbitmq/ path. The default access credentials are admin:admin.
     - Flower: Added Flower service on port 5555 for monitoring and debugging Celery. The dashboard is exposed via Nginx and is available at the /flower/ path. The default access credentials are admin:admin.
     - TUSd: Added TUSd server on port 1080 to support fault-tolerant uploads; Nginx is configured to proxy TUSd.
     - gx-it-proxy: Added gx-it-proxy service on port 4002 to support Interactive Tools.
   - Ansible Playbooks:
     - Migrated from galaxyextras git submodule to using mainatined ansible roles.
     - Added configure_rabbitmq_users.yml Ansible playbook, which removes the default guest user and adds admin, galaxy, and flower users for RabbitMQ during container startup.
   - Environment Variables:
     - Added `GUNICORN_WORKERS` and `CELERY_WORKERS` magic environment variables to set the number of Gunicorn and Celery workers, respectively, during container startup.
   - Configuration Changes:
     - Replaced the Galaxy Reports sample configuration file.
     - Removed galaxy_web, handlers, reports, and ie_proxy services from Supervisor.
     - Added Gravity for managing Galaxy services such as Gunicorn, Celery, gx-it-proxy, TUSd, reports, and handlers. It uses Supervisor as the process manager, with the configuration file located at /etc/galaxy/gravity.yml.
     - Added support for dynamic handlers (set as the default handler type).
     - Redis and Flower services are now managed by Supervisor.
     - Since Galaxy Interactive Environments are deprecated, they have been replaced by Interactive Tools (ITs). The sample configuration file tools_conf_interactive.xml.sample is placed inside GALAXY_CONFIG_DIR. Nginx is also configured to support both domain and path-based ITs.
     - Switched to using the cvmfs-config.galaxyproject.org repository for automatic configuration and updates of Galaxy project CVMFS repositories. Updated tool data table config path to include CVMFS locations from data.galaxyproject.org in --privileged mode.
     - Enabled IPv6 support in Nginx for ports 80 and 443.
     - Added Subject Alternative Name (SAN) extension (DNS:localhost and IP:127.0.0.1) while generating a self-signed SSL certificate.
     - Ensured the Nginx SSL certificate is trusted system-wide by adding it to the CA store.
     - Updated Galaxy extra dependencies.
     - Added docker_net, docker_auto_rm, and docker_set_user parameters for Docker-enabled job destinations.
     - Added update_yaml_value.py script to update nested key values in a YAML file.
     - Replaced ie_proxy with gx-it-proxy.
     - Replaced nginx_upload_module with TUSd for delegated uploads.
   - CI Tests
     - Added dive tool for analyzing the docker image
     - Added test for check data persistence
