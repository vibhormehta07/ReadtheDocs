.. include:: cyverse_rst_defined_substitutions.txt
.. include:: custom_urls.txt

|CyVerse_logo|_

|Home_Icon|_
`Learning Center Home <http://learning.cyverse.org/>`_


**BLAST a Transcriptome (Workflow Tutorial)**
==============================================

**Introduction and Overview**
-----------------------------
Author: Uwe Hilgert (hilgert at email.arizona.edu) / iPlant Collaborative, BIO5 Institute, University of Arizona

GOAL:
-----

The goal of this tutorial is to become familiar with a procedure to reduce the number of transcripts and the level of redundancy in an assembled transcriptome, and to identify coding sequences that can be submitted to BLASTP searches. This is a useful procedure to get an idea of the genes in a transcriptome set, without overwhelming the system by running time- and resource-intense BLASTX searches.

The tutorial will take users through the following operations:

 - Eliminate small transcripts (app: Select contigs)
 - Reduce transcript redundancy (app: CD-HIT-est 4.6.1)
 - Identify and translate coding sequences (app: Transcript decoder 1.0)
 - Submit translated transcriptome to BLASTP (app: Blastp-2.2.29+)
 - Alternatively: Submit translated transcriptome to Delta-BLAST (app: DeltaBLAST-2.2.29)*

* Another alternative search app (MPISSWIPE) is currently being generated and will be added here as soon as it becomes publicly available.

Quick Start Maintainer(s)
----------------------------

Who to contact if this quick start needs fixing. You can also email
`learning@CyVerse.org <learning@CyVerse.org>`_

.. list-table::
    :header-rows: 1

    * - Maintainer
      - Institution
      - Contact
    * - Vibhor Mehta
      - CyVerse / UA
      - vibhormehta@email.arizona.edu

----

**Rationale and Background**
-----------------------------

BLAST Query Size Transcriptomes assembled from Next-generation sequence data are often unnecessarily bloated by redundant contigs and/or contigs of sub-sufficient length. Reducing these unnecessary sequences can easily reduce the size of a transcriptome dataset to less than 50% of its original size and, thus, significantly reduce the time required for subsequent BLAST searches.

BLASTP versus BLASTX In an attempt to quickly get an idea about the genes represented in an assembled transcriptome, experimenters often use BLASTX to query their nucleotide set against a protein data base. The BLASTX algorithm, however, will translate the sequence in 3 reading frames in the forward direction and in 3 reading frames in the reverse direction to generate the amino acid sequences for the search. In essence, this process increases the size of the query six-fold, including all types of spurious (short) amino acid sequences that will increase the search time significantly. Identifying the coding sequences in an assembled and slimmed down contig set, and using the BLASTP algorithm for a straight protein data search will therefore significantly shorten the search time.

BLAST Search Space The final time-limiting search parameter is the size of the search space, the smaller the search space the faster the search. While experimenters might hope to find significantly more insights by searching the non-redundant, all-proteins containing "nr" database, they may reconsider that strategy when their results take a month to show. Instead, a little thought ahead of time may lead to an intelligent selection of a database or a handful of databases that may significantly reduce the time for the search. Commonly used databases include Refseq Protein, Refseq Protein minus Human and Mouse, UniprotKB, Uniref100, Invertebrate Proteins, and many others. Alternatively, users can generate and search their own, custom-made databases.

Prerequisite
~~~~~~~~~~~~~

1. An iPlant account. (Register for an iPlant account at user.iplantcollaborative.org.)
2. The DE Quick Start tutorial provides an introduction to basic DE functionality and navigation.

TestData
~~~~~~~~~
This tutorial uses Illumina sequencing data from a study in Belgica antarctica (Teets et al., 2012*) that are stored in the Data Store in the iPlant Discovery Environment. They were retrieved from the NCBI Sequence Read Archive (SRA) at http://www.ncbi.nlm.nih.gov/sra/?term=Belgica%20antarctica. To reduce processing times, the tutorial sample data set was reduced from the original number of 45,065 contigs to 200 contigs. (Conducting a BLASTP search using the original data set and searching RefSeq Protein would take approximately 9 days... and about six weeks using BLASTX.) These data as well as the results of each analysis step are provided in the iPlant Data Store following this path: Community Data > iplant_training > BLAST. A FASTQC conducted on these sequences indicated prior trimming and the data were not subjected to additional trimming but used as is. For instructions on how to evaluate and pre-treat short reads, visit the Evaluate and Pre-Process Sequencing Reads tutorial.

The sample BLAST search database consists of NCBI's RefSeq protein, excluding human and mouse sequences. The data were downloaded in July 2013 and are accessible at Community Data > iplantcollaborative > example_data > blastp > refseq_protein. RefSeq Protein is a comprehensive, integrated, non-redundant, well-annotated set of reference sequences derived from genomic, transcript, and protein data. For further information visit NCBI's RefSeq pages.

**Workflow**
------------

Operation 1: Eliminate small transcripts (app: Select contigs)
The Select contigs app utilizes the SOAPdenovo-Trans-1.0 tool, a de-novo transcriptome assembler that is adept to handling alternative splicing and different expression levels among transcripts. (Basic documentation: [http://soap.genomics.org.cn/SOAPdenovo-Trans.html])

1. Log into the Discovery Environment.
2. Open Select contigs (Apps > Public Apps > NGS > Assembly Annotation > Select contigs).
 a. Name your analysis.
 b. Select an "output folder." (Recommendation: create a "BLAST" directory in your DE > "analyses" directory.)
3. Click on the "Main Settings" tab.
 a. Provide for "Input fasta file" the path to your assembled transcripts file in FASTA format. (Sample data : Community Data > iplant_training > BLAST > 200_RNASeqContigs.fa)
 b. Enter an "Output fasta file name." (For the samples “200_RNASeqContigs_SC” was chosen.)
4. Click on the "Options" tab.
 a. Set the "Minimum contig size" to the desired minimum length. (For the sample data enter "300.")
5. Click on "Launch Analysis."
6. Once the analysis is completed (approx. 2 min. with the sample data), click on "Analysis," and then click on the analysis "Name" to open the output folder.
7. Examine the contents of the results folder by comparing the file size and the number of result pages between the original input and the output file, the output file should be smaller and contain less transcript reads than the input folder.
 a. By extracting from the sample input folder only transcripts with a minimum length of 300 bp, the folder size increased from 707 Kb to 713 Kb. However, this increase is due to a formatting issue, as the input file was significantly tightened by removing all unnecessary line breaks. The Select contigs app reduced the number of contigs from 200 to 179, or by about 10%. (Depending on the contig size distribution other data sets may be reduced more or less from this filtering step.)

 Operation 2: Reduce transcript redundancy (app: CD-HIT-est 4.6.1)
The CD-HIT-est 4.6.1 app compares sequence reads, clusters them according to their degree of sequence similarity and overlap and, finally, consolidates them by generating and retaining a representative sequence for each cluster. (Basic documentation: http://weizhong-lab.ucsd.edu/cd-hit/)
1. Open CD-HIT-est 4.6.1 (Apps > Public Apps > Assembly Annotation > CD-HIT-est 4.6.1).
 a. Name your analysis.
 b. Select your "output folder."
2. Click on the "Settings" tab.
 a. Provide for "Transcript fasta file" the path to your Select contigs-treated transcripts  file in FASTA format. (Sample data: Community Data > iplant_training > BLAST > 200_RNASeqContigs_SC.fa)
 b. Enter an "Output fasta file name." (For the samples “200_RNASeqContigs_CD-HITout” was chosen.)
    ##Select a "Global sequence identity" level. (For the sample data select 0.94 (94%).)
3. Click on "Launch Analysis."
4. Once the analysis is completed (approx. 5 min. with the sample data), click on "Analysis," and then click on the analysis "Name" to open the output folder.
5. Examine the contents of the results folder by comparing the file size and the number of result pages between the original input and the output file, the output file should be smaller and contain less transcript reads than the input folder.
 a.By limiting the sample input folder to unique transcripts of at least 94% similarity, the folder size was reduced from 713 Kb to 687 Kb. However, this condensation step reduced the number of contigs by 3 to 176, or by 1.7%. (Depending on the overlap among contigs other data sets may benefit more or less from this condensation step.)

 Operation 3: Identify and translate coding sequences (app: Transcript decoder 1.0)
 The Transcript decoder 1.0 app identifies candidate coding regions within transcript sequences, such as those generated by de-novo RNA-Seq transcript assembly using Trinity, or constructed based on RNA-Seq alignments to a genome using Tophat and Cufflinks. It generates output files that include the identified coding sequences, their matching peptide sequences, and gff and bed files that define the locations for the various transcripts and their exons within the transcript sequences. There are two versions for each of these files, one for all the coding sequences found, and another for “eclipsed” results, where coding sequences that were found within larger coding sequences have been removed as duplicates. (Basic documentation: http://transdecoder.sourceforge.net/)

 1. Open the Transcript decoder 1.0 app (Public Apps > NGS > Assembly Annotation > Transcript decoder 1.0)
  a. Name your analysis.
  b. Select your "output folder."
 2. Click on the "Main settings" tab.
  a. Provide for "transcript file input" the path to your Select contigs-treated transcripts  file in FASTA format. (Sample data: Community Data > iplant_training > BLAST > 200_RNASeqContigs_CD-HITout)
  b. Define a "Minimum ORF size." (Set to "300" for the sample data. This is equivalent to a peptide sequence of 100 amino acids.)
 3. Click on the "options" tab.
  a. Enter a "Minimum protein length." (For the sample data set to "100.")
 4. Click on "Launch Analysis."
 5. Once the analysis is completed (approx. 12 min. with the sample data), click on "Analysis," and then click on the analysis "Name" to open the output folder.
 6. Examine the contents of the results folder.
  a. Running the Transcript decoder 1.0 produced 14 files and a "logs" directory. The result files have descriptive names, which indicate their contents. They mainly consist of FASTA and text files that parse the output in various ways, including files with and without amino acid sequences, with and without coding sequences, and information that can be piped into various subsequent analysis routines. The BLASTP searches below will be conducted with the file named  "best_candidates.eclipsed_orfs_removed.pep." It contains 283 peptide sequences and not nucleotide sequences. The reason for it to contain a larger number of sequences than the 176 transcript sequences provided in the input file is that the many of the input transcripts contained several open reading frames of the minimum defined length of 300 nucleotides/100 amino acids.

Operation 4: Submit translated transcriptome to BLASTP (app: Blastp-2.2.29+)
BLASTP programs search protein databases using a protein query. The Blastp-2.2.29+ app utilizes the peptide database generated by the Transcript decoder 1.0 app and generates a set of search matches that provide a view onto the genes represented in the transcriptome. (Basic documentation: http://blast.ncbi.nlm.nih.gov/Blast.cgi?CMD=Web&PAGE_TYPE=BlastDocs&DOC_TYPE=ProgSelectionGuide)

1. Open Blastp-2.2.29+ (Apps > Public Apps > BLAST > Blastp-2.2.29+)
  a. Name your analysis.
  b. Select your "output folder."
2. Click on the "Inputs" tab.
  a. Provide for "Query sequence" the path to your Transcript decoder 1.0-generated peptides file in FASTA format. (Sample data are being provided in Community Data > iplant_training > BLAST > best_candidates.eclipsed_orfs_removed.pep.)
  b. Provide for "Database" the path to the database you wish to query. (For the sample data enter the path to the RefSeq proteins directory in Community Data > iplantcollaborative > example_data > blastp > refseq_protein.)
3. Click on the "Options" tab.
  a. Set options as desired. Note that this includes setting a cut-off for "e value." Depending on whether the data are derived from a species for which lots of data are in the databases or not, it may be of interest to enter a lower e value than the default of "10." While an e value of "0.01" may not lessen processing time, it could reduce analysis time by limiting the results to more meaningful search hits.
4. Click on "Launch Analysis."
5. Once the analysis is completed (approx. 4 hrs. with the sample data), click on "Analysis," and then click on the analysis "Name" to open the output folder.
6. Examine the contents of the results folder.
  a. Depending on options entered into  the app "options" tab, Blastp-2.2.29+ produces one or more output file that contains the search results to somewhat varying extents. Using the default settings with the sample data will produce one output file of about 125 Mb size. (A BLASTP search using the entire transcriptome data set of 45,065 transcripts, after size filtering, condensation and transcript decoding, took 10 days and yielded a 4.5 Gb output file with 23,114 peptide matches.)
  b. A quick analysis of the file shows that BLASTP identified significant alignments for 197 of the 283 input sequences; hits were not found for 86 sequences.
  c. Most hits were derived from insects.
  d. The e values for most hits were sufficiently small to indicate that the hit sequences were identified not due to random matching but due to homology with the respective query sequences.
  e. Comparing the bit scores with the lengths of the queries revealed that a good number of hits matched the queries fully, while others represented partial matches. This is rather unsurprising as there is little reason to assume that the RefSeq Protein database searched here would represent the B. antarctica proteome well. Consequently, most hits are derived from well-represented, but distant relatives of B. antarctica, such as Drosophila and Anopheles. The limited similarity between these hits and the B. antarctica  query sequences therefore explain well the discrepancies between bit scores and query lengths.

Alt. Operation 4: Submit translated transcriptome to Delta-BLAST (app: DeltaBLAST-2.2.29+)
Delta-BLAST (Domain Enhanced Lookup Time Accelerated BLAST; basic documentation: http://www.biologydirect.com/content/7/1/12) searches protein databases using a peptide sequence as query. In contrast to BLASTP it utilizes a separate database of conserved domains to increase the search sensitivity to detect remote protein homologs. This can be useful if the peptides being blasted are from a species for which otherwise not many data are available in current protein databases.

1. OpenDeltaBLAST-2.2.29+ (Apps > Public Apps > BLAST > DeltaBLAST-2.2.29+)
  a. Name your analysis.
  b. Select your "output folder."
2. Click on the "Inputs" tab.
  a. Provide for "Query sequence" the path to your Transcript decoder 1.0-generated peptides file in FASTA format. (Sample data are being provided in Community Data > iplant_training > BLAST > best_candidates.eclipsed_orfs_removed.pep.)
  b. Provide for "Database" the path to the database you wish to query. (For the sample data enter the path to the RefSeq proteins directory in Community Data > iplantcollaborative > example_data > blastp > refseq_protein.)
3. Click on the "Options" tab.
  a. Set options as desired. Note that this includes setting a cut-off for "e value." Depending on whether the data are derived from a species for which lots of data are in the databases or not, it may be of interest to enter a lower e value than the default of "10." While an e value of "0.01" may not lessen processing time, it could reduce analysis time by limiting the results to more meaningful search hits.
4. Click on "Launch Analysis."
5. Once the analysis is completed (approx. 7 hrs. with the sample data), click on "Analysis," and then click on the analysis "Name" to open the output folder.
6. Examine the contents of the results folder.
  a. Depending on options entered into  the app "options" tab, DeltaBLAST-2.2.29+ produces one or more output file that contains the search results to somewhat varying extents. Using the default settings with the sample data will produce one output file of about 970 Mb size. (A Delta-BLAST search with the entire transcriptome data set of 45,065 transcripts after size filtering, condensation and transcript decoding took 21 days and yielded an output file of approx. 14.7 Gb, and 207,465 matches)
  b. The Delta-BLAST algorithm returns a file that is about 6 times the size of the BLASTP file. This is due to the increased sensitivity of Delta-BLAST which is able to detect more distantly related protein homologs. It achieves this by first using the query sequences to identify conserved domains. It then constructs a position-specific score matrix to conduct a BLAST search over the defined search space (here: RefSeq Protein).
  c. A quick analysis of the results shows that Delta-BLAST identified significant alignments for 253 of the 283 input sequences; the number of query sequences for which hits were not found was reduced to 30 sequences as opposed to 86 using BLASTP.
  d. The results for Delta-BLAST list the hits that were identified with BLASTP. Beyond that, they contain a large number of hits that lead to more fragmented alignments and were not identified by BLASTP.

REFERENCE:
~~~~~~~~~~
  Sample data: *Nicholas M. Teets, Justin T. Peyton, Herve Colinet, David Renault, Joanna L. Kelley, Yuta Kawarasaki, Richard E. Lee, Jr, David L. Denlinger, Proc Natl Acad Sci U S A. 2012 December 11; 109(50): 20744--20749.

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
