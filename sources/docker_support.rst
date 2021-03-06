:title: Docker support
:description: Docker specific features supported on Shippable
:keywords: shippable, Docker, Container, dockerfile, dockerbuild, docker hub

.. _dockersupport:

DOCKER SUPPORT
==============
Shippable is the world's only CI/CD platform built natively on Docker. All builds are run on Docker containers and this gives us a unique ability to support advanced docker workflows. We're constantly adding to our custom Docker support, so check back often!

**Connect your Docker Hub account to Shippable**
------------------------------------------------

If you want to interact with Docker Hub in any part of your build workflow, you need to connect your Docker Hub account to Shippable. This is a requirement for pulling custom images from your Docker Hub repos or pushing images to Docker Hub.

You can set up Docker Hub integration on a per-Org basis.

1. Go to your Organization's page on Shippable. You can find your Organization name at the right side of your Dashboard and click on it to go to the Org page.
2. If the 'Docker Hub' icon at the top right of your Org page is set to ON, you have already connected your Docker Hub account to your Org. If it is set to OFF, click on it, enter your Docker Hub credentials, and click on Save.

We do not call the Docker Hub API until we need to do so during an actual build, so we have no way of knowing if your credentials are correct at this point. The 'Docker Hub' icon will turn green after you save your creds, but if you run into problems with login to Docker Hub during the build, you should check your creds again.


**push to Docker Hub**
----------------------

Shippable allows you to push an image to the docker registry after a successful build. To do this, make sure your Docker Hub icon is set to ON on your Organization's page on Shippable.

The following configuration in your shippable.yml will push the image to Docker Hub after the build is successful.

.. code-block:: bash

    commit_container: username/sample_project

The username above should be the same as the Docker Hub credentials you entered in the previous step.


**Dockerbuild**
---------------

.. note::
  Docker Build Support is only available with dedicated hosts. To set up a dedicated host, please follow instructions `here <http://docs.shippable.com/en/latest/config.html#dedicated-hosts>`_

You can run your build in a custom docker container by building a Docker image from a Dockerfile. Aside from providing a custom environment for your build, this image created can be pushed to your Docker Hub account, for later use in your deployment step.

There are 2 ways to set up Docker build with Shippable - pre CI or post CI. 

Pre CI workflow is:
1. Build the image using Dockerfile at the root of your repo
2. Pull code from GitHub/Bitbucket and test code in the container
3. Push container to docker hub

Post CI workflow is:
1. Pull image specified from Docker Hub (default is minv2)
2. Pull code from GitHub/Bitbucket and test in container
3. If CI passs, build container from Dockerfile at the root of the repo
4. Push container to docker hub

To use these workflows, your app must be "dockerized". Details on this can be found in Docker's official documentation `Docker's official documentation <https://docs.dockerhub.com>`_. You can also look at our `Docker build sample app <https://github.com/cadbot/dockerized-nodejs>`_. 


**Pre CI Dockerbuild**

1. Make sure your Shippable org is connected to your Docker Hub account
2. Enable the repository on Shippable
3. On the repo page, go to 'Settings'. Choose the following -
  - Build image : Custom Image
  - Custom image action : Build
  - Custom image name : (docker hub username)/(image name)
  - Source code path : (source code path for image you want to build)
  - Push to Docker Hub : Check
4. Make sure the Dockerfile for the image you want to build is at the root of your repo
5. Trigger a manual or webhook build
6. After the build is complete, make sure your Docker Hub account has the image you just pushed. The image should be tagged with the build number on Shippable.

**Post CI Dockerbuild**

1. Make sure your Shippable org is connected to your Docker Hub account
2. Enable the repository on Shippable
3. On the repo page, go to 'Settings'. Choose the following -
  - Build image : Custom Image
  - Custom image action : Build
  - Custom image name : (docker hub username)/(image name)
  - Source code path : (source code path for image you want to build)
  - Docker build when finished : Check
  - Image to pull: Specify image you want to run tests on, default is shippable/minv2
  - Push to Docker Hub : Check
4. Make sure the Dockerfile for the image you want to build is at the root of your repo
5. Trigger a manual or webhook build
6. After the build is complete, make sure your Docker Hub account has the right image. The image should be tagged with the build number on Shippable.

**Copying artifacts to prod image**

If you are following the post-CI Dockerbuild workflow and  want to copy some build artifacts to your prod image, you should-
1. create a shippable/buildoutput directory in your shippable.yml

.. code-block:: bash

  before_script:
    - mkdir -p shippable/buildoutput

2. in the after_script section, copy whatever you want to this directory

.. code-block:: bash

  after_script:
    - cp -r (your artifacts) ./shippable/buildoutput

3. In your Dockerfile, you can now use ADD to put the artifacts wherever you want in your prod image

.. code-block:: bash

  ADD ./buildoutput/(artifacts file) (target)

And that's it. Any artifacts you need will be available in your prod image.
