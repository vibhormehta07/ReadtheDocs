.. include:: cyverse_rst_defined_substitutions.txt

|CyVerse logo|_

|Home_Icon|_
`Learning Center Home <http://learning.cyverse.org/>`_

**How to Integrate a Tool in DE**
=================================

Goal
----
This tutorial will help you understand the process and how a tool can be integrated in the
Discovery Environment using Github and Docker.

Prerequisites
-------------

Downloads, access, and services
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*In order to complete this tutorial you will need access to the following services/software*

 .. list-table::
   :header-rows: 1

   * - Prerequisite
     - Preparation/Notes
     - Link/Download
   * - CyVerse account
     - You will need a CyVerse account to complete this exercise
     - |CyVerse User Portal|
   * - Docker account
     - You need your docker account to add the tool to your docker account
     - `Docker login Portal <https://id.docker.com/login//>`_

Platform(s)
~~~~~~~~~~~

*We will use the following CyVerse platform(s):*

.. list-table::
    :header-rows: 1

    * - Platform
      - Interface
      - Link
      - Platform Documentation
      - Quick Start
    * - Discovery Environment
      - Web/Point-and-click
      - |Discovery Environment|
      - |DE Manual|
      - |Discovery Environment Guide|


*Steps to Follow*
-----------------

1. Create a docker container that contains your tool

 1.1 You can create docker container by first downloading docker on your machine

 1.2 To check if docker is installed type docker help in command line or terminal

 .. code-block:: bash

     docker help

2. Once docker displays all the commands it means that it is working

 2.1 Now create a folder in your machine and then create the "Dockerfile"

 .. code-block:: bash

     mkdir Rstudio-docker && cd Rstudio-docker
     nano Dockerfile

 2.2 Dockerfile is the basic text file that will be the base of your docker image. So here is an example of Rstudio "Dockerfile"

 .. code-block:: bash

    FROM rocker/rstudio:3.5.2

    RUN apt-get update \
    && apt-get install -y lsb wget apt-transport-https python2.7 python-requests curl supervisor nginx gnupg2

    RUN wget -qO - https://packages.irods.org/irods-signing-key.asc | apt-key add - \
    && echo "deb [arch=amd64] https://packages.irods.org/apt/ xenial main" > /etc/apt/sources.list.d/renci-irods.list \
    && apt-get update \
    && apt-get install -y irods-icommands

    ADD https://github.com/hairyhenderson/gomplate/releases/download/v2.5.0/gomplate_linux-amd64 /usr/bin/gomplate
    RUN chmod a+x /usr/bin/gomplate

    COPY run.sh /usr/local/bin/run.sh
    COPY nginx.conf.tmpl /nginx.conf.tmpl
    COPY rserver.conf /etc/rstudio/rserver.conf
    COPY supervisor-nginx.conf /etc/supervisor/conf.d/nginx.conf
    COPY supervisor-rstudio.conf /etc/supervisor/conf.d/rstudio.conf

    ENV REDIRECT_URL "http://localhost"
    ENV PASSWORD "rstudio1"

    RUN bash /etc/cont-init.d/userconf

    ENTRYPOINT ["/usr/local/bin/run.sh"]



 2.3 Once you create the "Dockerfile", you need to build the container

 .. code-block:: bash

     docker build -t rstudio .

 2.4 Test the docker image by typing the following command

 .. code-block:: bash

     docker run rstudio



 2.5 Now tag the image with your dockerhub-username followed by the image name.

 .. code-block:: bash

    docker tag rstudio username/rstudio

 2.6 Now login and then push the container to DockerHub where it can be accessed by others

  .. code-block:: bash

      docker login -u username
      docker push username/rstudio

3. Login to the `discovery_environment <https://de.cyverse.org/de/>`_

 3.1 Go to Apps on the left hand corner

|De app|

 3.2 Click on manage tools in the top ride side of the window

 3.3 Click on Tools --> Add tool


 3.4 Enter name you want to give your tool and fill the form with the required fields

|De create tool|

3.5 Below are specific ports for vice tools in DE which need to be entered as ENTRYPOINTS based on the tool you are creating.


  +------------+---------+
  | Type       | Port    |
  +------------+---------+
  | Jupyter    | 8888    |
  +------------+---------+
  | Rstudio    | 80      |
  +------------+---------+
  | Shiny      | 3838    |
  +------------+---------+


*Summary*
~~~~~~~~~~~

**Next Steps:**

The above tutorial can be repeated to add tools of your choice by just replacing certain
like container name, login id , tool name.
Some common next steps include:

1. Always have a Dockerfile before trying to build your container


----

Additional information, help
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
    Short description and links to any reading materials

Search for an answer:
|CyVerse Learning Center| or
|CyVerse Wiki|

Post your question to the user forum:
|Ask CyVerse|

----

**Fix or improve this documentation**

- On Github: |Github Repo Link|
- Send feedback: `Tutorials@CyVerse.org <Tutorials@CyVerse.org>`_

----

|Home_Icon|_
`Learning Center Home <http://learning.cyverse.org/>`__

.. Comment: Place Images Below This Line
   use :width: to give a desired width for your image
   use :height: to give a desired height for your image
   replace the image name/location and URL if hyperlinked


 .. |Clickable hyperlinked image| image:: ./img/IMAGENAME.png
    :width: 500
    :height: 100
 .. _CyVerse logo: http://learning.cyverse.org/

 .. |Static image| image:: ./img/IMAGENAME.png
    :width: 25
    :height: 25

.. |De app| image:: ./img/apps.png

.. |De create tool| image:: ./img/imgrstudio.png

.. |De tool name| image:: ./img/imgrstudio2.png


.. Comment: Place URLS Below This Line

   # Use this example to ensure that links open in new tabs, avoiding
   # forcing users to leave the document, and making it easy to update links
   # In a single place in this document

   .. |Substitution| raw:: html # Place this anywhere in the text you want a hyperlink

      <a href="REPLACE_THIS_WITH_URL" target="blank">Replace_with_text</a>


.. |Github Repo Link|  raw:: html

   <a href="FIX_FIX_FIX_FIX_FIX_FIX_FIX_FIX_FIX_FIX_FIX_FIX_FIX_FIX_FIX" target="blank">Github Repo Link</a>


.. |Download Cyberduck| raw:: html

   <a href="https://cyberduck.io/" target="blank">Download Cyberduck</a>
