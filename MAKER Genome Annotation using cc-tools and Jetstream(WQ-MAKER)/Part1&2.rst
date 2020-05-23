.. include:: cyverse_rst_defined_substitutions.txt

|CyVerse logo|_

|Home_Icon|_
`Learning Center Home <http://learning.cyverse.org/>`_

**MAKER Genome Annotation using cc-tools and Jetstream(WQ-MAKER)**
==================================================================

**Part 1: Connect to an instance of an WQ-MAKER Jetstream Image (virtual machine)**

Step 1: Go to Jetstream_ and log in with your XSEDE credentials.

|WQ-Maker1|

|WQ-Maker2|

|WQ-Maker3|

Step 2: Click on the "Create New Project"  in the Project tab on the top and enter the name of the project and a brief description

|WQ-Maker4|

|WQ-Maker5|

Step 3:  Launch an instance from the selected image and name it as MASTER

After the project has been created and entered inside it, click the "New" button, select "MAKER 2.31.9 with CCTools" image and then click Launch instance. In the next window (Basic Info),

|WQ-Maker6|

     3.1 name the instance as "MASTER" (don't worry if you forgot to name the instance at that point, as you can always modify the name of the instance later)
     3.2 set base image version as "2.0" (default)
     3.3 leave the project as it is or change to a different project if needed
     3.4 select "Jetstream - Indiana University or Jetstream - TACC" as Provider and click 'Continue'. Your choice of provider will depend on the resources you have available (AUs) and the needs of your instance
     3.5 select "m1.medium" as Instance size (this is the minimum size that is required by WQ-MAKER image) and click "Continue".

|WQ-Maker7|

Step 4: As the instance is launched behind the scenes, you will get an update as it goes through each step.

Status updates of Instance launch (both MASTER and WORKER) include Build-requesting launch, Build-networking, Build-spawning, Active-networking, Active-deploying. Depending on the usage load on Jetstream, it can take anywhere from 2-5 mins for an instance to become active. You can force check updates by using the refresh button in the Instance launch page or the refresh button on your browser. Once the instance becomes active a virtual machine with the ip address provided will become available for you to connect to. This virtual machine will have all the necessary components to run WQ-MAKER and test files to run a MAKER demo.

|WQ-Maker8|

Step 5: Create a volume

Since the m1 medium instance size (60GB disk space) selected for running MASTER instance of WQ-MAKER may not be sufficient for most of the MAKER runs, it is recommended to run it on volumes

5.1 Click the "New" button in the project and select "Create Volume". Enter the name of the volume, volume size (GB) needed and the provider (TACC or Indiana) and finally click "Create Volume"

|WQ-Maker9|

Attach the created volume to the MASTER Instance

|WQ-Maker10|

5.2 Click on the MASTER instance now

|WQ-Maker11|

Jetstream provides web-shell, a web based terminal, for accessing your VM at the command line level once its been deloyed.

|WQ-Maker12|

However, you might find that you wish to access your VM via SSH if you’ve provisioned it with a routable IP number. For SSH access, you can create (or copy) SSH public-keys for your non-Jetstream computer that will allow it to access Jetstream then deposit those keys in your Atmosphere settings. More instructions can be found here

.. code-block:: bash

    $ ssh <username>@<ipaddress>


Step 6: Add public SSH key of MASTER to Jetstream

If you do not already have a ~/.ssh/id_rsa.pub file, then run this command to create it. Use all the defaults..

.. code-block:: bash

    $ ssh-keygen
    Generating public/private rsa key pair.
    Enter file in which to save the key (/home/upendra/.ssh/id_rsa):
    Enter passphrase (empty for no passphrase):
    Enter same passphrase again:
    Your identification has been saved in /home/upendra/.ssh/id_rsa.
    Your public key has been saved in /home/upendra/.ssh/id_rsa.pub.
    The key fingerprint is:
    SHA256:fPvsPyvtcWKl9kQotmJskFljlHQNCNP2pITigdLS3mM upendra@js-157-180.jetstream-cloud.org
    The key's randomart image is:


        +---[RSA 2048]----+
        |   o .  o=oooo   |
        |  o + o .o*.. .  |
        |   + o o o++     |
        |    . E. =...  . |
        |     . .S . o . o|
        |         + o o + |
        |          * ..* o|
        |         o +.oo* |
        |           .+++o.|
        +----[SHA256]-----+



Copy the public SSH key from your id_rsa.pub file and paste it to the cloudsettings_, give a name (MASTER) to it and click confirm.

.. code-block:: bash
    cat ~/.shh/id_rsa.pub


|WQ-Maker13|

Step 7: Launch WORKER instances from MAKER 2.31.9 with CCTools image

Launch one to several instances from the MAKER 2.31.9 with CCTools image and name them as WORKER-1, WORKER-2 etc.,

|WQ-Maker14|


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

.. _Jetstream: https://use.jetstream-cloud.org/application
.. _cloudsettings: https://use.jetstream-cloud.org/application/settings

.. |WQ-Maker1| image:: ./img/WQ-Maker1.png

.. |WQ-Maker2| image:: ./img/WQ-Maker2.png

.. |WQ-Maker3| image:: ./img/WQ-Maker3.png

.. |WQ-Maker4| image:: ./img/WQ-Maker4.png

.. |WQ-Maker5| image:: ./img/WQ-Maker5.png

.. |WQ-Maker6| image:: ./img/WQ-Maker6.png

.. |WQ-Maker7| image:: ./img/WQ-Maker7.png

.. |WQ-Maker8| image:: ./img/WQ-Maker8.png

.. |WQ-Maker9| image:: ./img/WQ-Maker9.png

.. |WQ-Maker10| image:: ./img/WQ-Maker10.png

.. |WQ-Maker11| image:: ./img/WQ-Maker11.png

.. |WQ-Maker12| image:: ./img/WQ-Maker12.png

.. |WQ-Maker13| image:: ./img/WQ-Maker13.png

.. |WQ-Maker14| image:: ./img/WQ-Maker14.png
