:title: Custom Containers with Docker Build
:description: Running minions in a Docker container defined by a Dockerfile
:keywords: shippable, Docker, Container

.. _docker_build:

.. note::
  Docker Build Support is with dedicated hosts only!

DOCKER REGISTRIES
=================
Shippable is the world's only CI/CD platform built natively on Docker. All builds are run on Docker containers and this gives us a unique ability to support advanced Docker workflows. We're constantly adding to our custom Docker support, so check back often!

**Docker Hub**
--------------

**Connect your Docker Hub account to Shippable**

If you want to interact with Docker Hub in any part of your build workflow, you need to connect your Docker Hub account to Shippable. This is a requirement for pulling custom images from your Docker Hub repos or pushing images to Docker Hub.

You can set up Docker Hub integration on a per-Org basis.

1. Go to your Organization's page on Shippable. You can find your Organization name at the right side of your Dashboard and click on it to go to the Org page.
2. If the 'Docker Hub' icon at the top right of your Org page is set to ON, you have already connected your Docker Hub account to your Org. If it is set to OFF, click on it, enter your Docker Hub credentials, and click on Save.

We do not call the Docker Hub API until we need to do so during an actual build, so we have no way of knowing if your credentials are correct at this point. The 'Docker Hub' icon will turn green after you save your creds, but if you run into problems with login to Docker Hub during the build, you should check your creds again.

-------

**Push to Docker Hub**

Shippable allows you to push an image to the docker registry after a successful build. To do this, make sure your Docker Hub icon is set to ON on your Organization's page on Shippable.

On the repository page, choose the following options to push to Docker Hub -

* Push Build : Yes
* Push image to : docker_hub_username/image_name

The username above should be the same as the Docker Hub credentials you entered in to connect your Docker Hub account to Shippable.

-------

**Dockerbuild**

.. note::
  Docker Build Support is only available with dedicated hosts. To set up a dedicated host, please follow instructions `here <http://docs.shippable.com/en/latest/config.html#dedicated-hosts>`_

You can run your build in a custom Docker container by building a Docker image from a Dockerfile. Aside from providing a custom environment for your build, this image created can be pushed to your Docker Hub account, for later use in your deployment step.

There are 2 ways to set up Docker build with Shippable - pre CI or post CI. 

Pre CI workflow is:

* Build the image using Dockerfile at the root of your repo
* Pull code from GitHub/Bitbucket and test code in the container
* Push container to Docker Hub

Post CI workflow is:

* Pull image specified from Docker Hub (default is minv2)
* Pull code from GitHub/Bitbucket and test in container
* If CI passes, build container from Dockerfile at the root of the repo
* Push container to Docker Hub

To use these workflows, your app must be "Dockerized". Details on this can be found in Docker's official documentation `Docker's official documentation <https://docs.docker.com/docker-hub/>`_. You can also look at our `Docker build sample app <https://github.com/cadbot/dockerized-nodejs>`_. 

**Pre CI Dockerbuild**

* Make sure your Shippable org is connected to your Docker Hub account
* Enable the repository on Shippable
* On the repo page, go to 'Settings'. Choose the following -

  * Docker Build : ON
  * Dockerbuild order : Pre-CI
  * Push Build : Yes if you want to push to Docker Hub, No if you don't want to push to Docker Hub 
  * Push image to : docker_hub_username/image_name  
    We need an image name for the image we build from your Dockerfile, even if you choose not to push to GCR
  * Source Location : (source code location where tests will be run)

* Make sure the Dockerfile for the image you want to build is at the root of your repo
* Trigger a manual or webhook build
* After the build is complete, make sure your Docker Hub account has the image you just pushed. The image should be tagged with the build number on Shippable.

**Post CI Dockerbuild**

* Make sure your Shippable org is connected to your Docker Hub account
* Enable the repository on Shippable
* On the repo page, go to 'Settings'. Choose the following -

  * Docker Build : ON
  * Dockerbuild order : Post-CI
  * Push Build : Yes if you want to push to Docker Hub, No if you don't want to push to Docker Hub 
  * Push image to : docker_hub_username/image_name  
  * Pull image from : Since your Dockerbuild is happening post CI, enter the image you want to use for CI

* Make sure the Dockerfile for the image you want to build is at the root of your repo
* Trigger a manual or webhook build
* After the build is complete, make sure your Docker Hub account has the right image. The image should be tagged with the build number on Shippable.

**Copying artifacts to prod image**

If you are following the post-CI Dockerbuild workflow and  want to copy some build artifacts to your prod image, you should-

1. Create a shippable/buildoutput directory in your shippable.yml

.. code-block:: bash

  before_script:
    - mkdir -p shippable/buildoutput

2. In the after_script section, copy whatever you want to this directory

.. code-block:: bash

  after_script:
    - cp -r (your artifacts) ./shippable/buildoutput

3. In your Dockerfile, you can now use ADD to put the artifacts wherever you want in your prod image

.. code-block:: bash

  ADD ./buildoutput/(artifacts file) (target)

And that's it. Any artifacts you need will be available in your prod image.

