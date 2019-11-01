# crc32-checksum-file-generator-java
This is a JAVA code which generates Linux CRC 32 checksum file using Jacksum jar
Audit Files and Usage of Dropbox Inbound Folder
Files dropped to the /inbound directory of the drop box will not get picked up for processing unless a corresponding audit file is present. This article will explain what an audit file is and how to generate one.
 Solution/Workaround:
An audit file is a file containing the output of the cksum UNIX command. This can be generated on UNIX systems or on Windows system using cygwin. The audit file filename is the same as the input filename but with an .aud added to the end of it.
 
Here is a description of the cksum command from the manual pages of a UNIX system:
~~~ 
DESCRIPTION
     The cksum command calculates and writes to standard output a
     cyclic redundancy check (CRC) for each input file, and also
     writes to standard output the number of octets in each file.
 ~~~
 Below is an example of a user generating an audit file in a UNIX system:
 ~~~
[user@host ~]$ ls ABCD*
ABCD_OGPO_DEV_20160414_100000_test.txt
[user@host ~]$ cksum ABCD_OGPO_DEV_20160414_100000_test.txt > ABCD_OGPO_DEV_20160414_100000_test.txt.aud
[user@host ~]$ cat ABCD_OGPO_DEV_20160414_100000_test.txt.aud
4294967295 5125 ABCD_OGPO_DEV_20160414_100000_test.txt
 ~~~
In the above example, the 4 character customer ID is ABCD. Once the data file and audit file are generated, the files will look like this:
ABCD_OGPO_DEV_20160414_100000_test.txt and ABCD_OGPO_DEV_20160414_100000_test.txt.aud could both be uploaded to the /inbound folder
As best practice, make sure the data file gets uploaded before the audit file. This helps avoid issues with large files being picked up too early, which can cause the below type of message in the in-bound ODI email:
[ProcessDataFiles] - ERROR: ABCD_OGPO_DEV_20160414_100000_test.txt CHECKSUM FAILED
[ProcessDataFiles] - ERROR: Audit Checksum: 3412
[ProcessDataFiles] - ERROR: Data File Checksum: 2328460807
