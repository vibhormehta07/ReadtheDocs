.. include:: cyverse_rst_defined_substitutions.txt
.. _Part1&2: Part1&2.rst
|CyVerse logo|_

|Home_Icon|_
`Learning Center Home <http://learning.cyverse.org/>`_

**MAKER Genome Annotation using cc-tools and Jetstream(WQ-MAKER)**
==================================================================

Rationale and background:
-------------------------
MAKER is a flexible and scalable genome annotation pipeline that automates the many steps necessary for the detection of protein coding genes (Campbell et al. 2013). MAKER identifies repeats, aligns ESTs and proteins to a genome, produces ab initio gene predictions, and automatically synthesizes these data into gene annotations having evidence-based quality indices.  MAKER was developed by the Yandell Lab and is described in several publications (Cantarel et al. 2008; Holt & Yandell 2011).  Additional background is available at the MAKER Tutorial at GMOD and is highly recommended reading.

MAKER with CCTools (aka WQ-MAKER) is a modified MAKER annotation tools capable of running MAKER on distributed computing resources such as Jetstream cloud (Thrasher et al., 2012). Using the work-queue platform, users can now run MAKER across multiple virtual machines to achieve a several fold reduction in the duration of the MAKER run.

This tutorial will take users through steps of:

1. Running WQ-MAKER on Jetstream cloud
2. Running WQ-MAKER on an example genome assembly data

Considerations
--------------
**Sounds great, what do I need to get started?**

1. Jetstream allocation. If you don't have one, you can send in your request to add you to CyVerse's JS allocation through the intercom (button on the bottom right on this page).
2. XSEDE account
3. Your data (or you can run example data)

**What kind of data do I need?**

1. Mandatory Requirements

 1.1 Genome assembly(fasta file)

 1.2 Organism Type

   1.2.1 Eukaryotic(default, set as: organism_type=eukaryotic)
   1.2.2 Prokaryotic(default, set as: organism_type=prokaryotic)

2. Additional data that can be used to improve the annotation (Highly recommended)

 2.1 RNA evidence (at least one of them is needed)

   2.1.1 Assembled mRNA-seq transcriptome (fasta file)

   2.1.2 Expressed sequence tags (ESTs) data (fasta file)

   2.1.3 Aligned EST or transcriptome GFF3 from your organism

   2.1.4 Aligned EST or transcriptome GFF3 from a closely related organism

 2.2 Protein evidence

   2.2.1 protein sequence file in fasta format (i.e. from multiple organisms)

   2.2.2 protein gff (aligned protein homology evidence from an external GFF3 file)

**What kind of resources will I need for my project?**

1. Enough storage space on the WQ-MAKER Jetstream instance for both input and output files
 1.1 Creating and mounting an external volume to the running WQ-MAKER MASTER instance would be recommended

2. One Master and several workers needed for running your computation
 2.1 Benchmarking results for data sets can help you estimate the number of workers need for running your annotation

3. Enough AUs to run your computation

Part1&2__

**Part 4: Set up a MAKER run using the Jupyter notebook**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Step1: Running "tmux" command to run the anlaysis in the background:

.. code-block:: bash

    $ cd ~
    $ tmux

Step2: Copy the example Jupyter notebook onto your home directory:

.. code-block:: bash

    $ cp /opt/WQ-MAKER_example_data/WQ-MAKER-Jupyter-notebook.ipynb .

Step3: Launch jupyter notebook in the background:

.. code-block:: bash

    $ python /opt/anaconda2/bin/jupyter-notebook --no-browser --ip=0.0.0.0 2>&1 | sed s/0.0.0.0/$(curl -s ipinfo.io/ip)/g
    [I 16:54:10.440 NotebookApp] Writing notebook server cookie secret to /run/user/1000/jupyter/notebook_cookie_secret
    [I 16:54:10.650 NotebookApp] Serving notebooks from local directory: /home/upendra
    [I 16:54:10.650 NotebookApp] 0 active kernels
    [I 16:54:10.650 NotebookApp] The Jupyter Notebook is running at:`Jupyternotebookrunninglink <http://129.114.104.169:8888/?token=483a27cd0387ccd04133570999ba8ce8072cf0f45663289f>`__
    [I 16:54:10.650 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
    [C 16:54:10.650 NotebookApp]

        Copy/paste this URL into your browser when you connect for the first time,
        to login with a token:
            `Copylinkbelow <http://129.114.104.169:8888/?token=483a27cd0387ccd04133570999ba8ce8072cf0f45663289f>`__ ## Copy and paste this link in the browser

Step 4: Now click open "WQ-MAKER-Jupyter-notebook.ipynb" and run the jupyter notebooks.

|WQ-Maker15|

Here is the example Jupyter notebook that was ran before


`Jupyter Example <http://nbviewer.jupyter.org/github/upendrak/WQ-MAKER_example_data/blob/master/WQ-MAKER-Jupyter-notebook-demo.ipynb>`__

**Note**
~~~~~~~~~
If you want to exit out of tmux shell without killing it. Press ctrl + b and d. If you want to kill it. Then press ctrl +c

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


.. |WQ-Maker15| image:: ./img/WQ-Maker15.png
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

.. |Part1&2| file:: ./Part1&2.rst
