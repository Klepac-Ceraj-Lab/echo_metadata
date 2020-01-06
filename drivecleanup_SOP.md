# Cleaning up, organizing, and archiving in the `echo` drives

```sh
# external echo drive map
## same across all drives (ntm, franklin, lovelace)
echo/
  analysis/
    biobakery/
    engaging/
    gutbrain/
  attic/
    analysis/
    databases/
    sequencing/
  databases/
    master_fecal_samples.csv
    YYYYMMDD_deidentified.fmp12
  sequencing/
    16S/
    mgx/
      batch###_files.txt
      details/
        batch###.md5
        batch###_details.txt
      rawfastq/
      submissions/
        YYYYMMDD_batch###_mgx.xlsx
    mgx-old/
```

## rsync changes across drives

```sh
$ ssh rosalind
$ rsync -avzP /Volumes/vkclab/echo/databases/ /Volumes/franklin/echo/databases/
$ rsync -avzP /Volumes/vkclab/echo/databases/ ada:/lovelace/echo/databases/
```

Want to rsync `/lovelace/echo/sequencing/` with `/Volumes/echo/sequencing/` and `/Volumes/vkclab/echo/sequencing` and `echo/attic/sequencing`. What `rsync` options should I use?

`-avzP` = archive, verbose, compress, show progress

**`-n` = dry-run!!!**

1. Rsync `lovelace/echo/attic/sequencing` to franklin and ntm
  - Copy what's in lovelace and not delete what's in franklin and ntm

```sh
  $ ssh rosalind
  $ rsync -avzP ada:/lovelace/echo/attic/sequencing/ /Volumes/vkclab/echo/attic/sequencing/
  $ rsync -avzP ada:/lovelace/echo/attic/sequencing/ /Volumes/franklin/echo/attic/sequencing/
```
```sh
# Differences between echo/attic/sequencing/
# /lovelace/echo/
ls -l attic/sequencing/
total 360
-rw-rw-r-- 1 sophie facstaff    165 Jul 27  2018 '~$IMR-SampleSubmissionSheet_v8(Rowland_May17).xlsx'
-rw-rw-r-- 1 sophie facstaff   5865 Jul 26  2018  ECHO_May2018_mapping.txt
-rw-rw-r-- 1 sophie facstaff  16306 Jul 26  2018  ECHO_May2018_mapping.xlsx
-rw-rw-r-- 1 sophie facstaff 261114 Jul 18  2018 'February 2018 Illumina & Metagenome Sequencing.xlsx'
-rw-rw-r-- 1 sophie facstaff  35359 Jul 18  2018 'IMR-SampleSubmissionSheet_v8(Rowland_Feb20).xlsx'
-rw-rw-r-- 1 sophie facstaff  38317 Jul 18  2018 'IMR-SampleSubmissionSheet_v8(Rowland_May17).xlsx'
# /Volumes/vkclab/echo/
$ ls -l attic/sequencing/
# /Volumes/franklin/echo/
$ ls -l attic/sequencing/
total 0
drwxrwxr-x  10 sophie  staff  340 May 30  2019 weak_failed
```

2. Rsync `lovelace/echo/sequencing` to franklin and ntm
  - Copy folders within `sequencing/`
    - Copy `sequencing/oral_mgx`
  - Remove files that have been removed from `sequencing/mgx/`

```sh
# Copy /sequencing/oral_mgx
$ ssh rosalind
$ mkdir sequencing/oral_mgx  #on franklin and ntm
$ rsync -avzP ada:/lovelace/echo/sequencing/oral_mgx /Volumes/vkclab/echo/sequencing/oral_mgx
$ rsync -avzP ada:/lovelace/echo/sequencing/oral_mgx /Volumes/franklin/echo/sequencing/oral_mgx

# Copy /sequencing/mgx
$ rsync -avzP ada:/lovelace/echo/sequencing/mgx /Volumes/vkclab/echo/sequencing/mgx --delete
$ rsync -avzP ada:/lovelace/echo/sequencing/mgx /Volumes/franklin/echo/sequencing/mgx --delete
```
* -a -rlptgoD
* -r recursive
* -l links
* -p perms, preserve permissions
* -t times, preserve mod times
* -g group, preserve group
* -o owner, preserve owner
* -D devices, preserve device files

Not working possibly because of errors from transferring between a Linux system and a non-Linux system.

Trying a command to just delete unwanted files.

```sh
$ rsync -rvzPn --delete --existing --ignore-existing --ignore-errors ada:/lovelace/echo/sequencing/mgx /Volumes/vkclab/echo/sequencing/mgx
```

```sh
# Differences between echo/sequencing
# /lovelace/echo/
$ ls -l sequencing/mgx/
total 856
-rw-rw-r-- 1 sophie facstaff  23608 Apr 20  2019 batch001_files.txt
-rw-rw-r-- 1 sophie facstaff  28048 Apr 20  2019 batch002_files.txt
-rw-rw-r-- 1 sophie facstaff  22424 Apr 20  2019 batch003_files.txt
-rw-rw-r-- 1 sophie facstaff  17992 Apr 23  2019 batch004_files.txt
-rw-rw-r-- 1 sophie facstaff  28344 Apr 18  2019 batch005_files.txt
-rw-rw-r-- 1 sophie facstaff  19464 Jan 19  2019 batch006_files.txt
-rw-rw-r-- 1 sophie facstaff  19168 Mar 12  2019 batch007_files.txt
-rw-rw-r-- 1 sophie facstaff  28344 Apr  5  2019 batch008_files.txt
-rw-rw-r-- 1 sophie facstaff  38336 Jun  4  2019 batch009_files.txt
-rw-rw-r-- 1 sophie facstaff  28344 Sep 10 09:32 batch010_files.txt
-rw-rw-r-- 1 sophie facstaff  28344 Sep 18 17:11 batch011_files.txt
-rw-rw-r-- 1 sophie facstaff  28352 Oct 22 10:51 batch012_files.txt
drwxrwxr-x 2 sophie facstaff   4096 Oct 22 12:40 details
drwxrwxr-x 2 sophie facstaff 536576 Oct 22 13:56 rawfastq
-rw-rw-r-- 1 sophie facstaff    354 May 30  2018 README.rtf
drwxrwx--- 2 sophie facstaff   4096 Aug 28 12:27 submissions
# /Volumes/franklin/echo/
$ ls -l sequencing/mgx/
total 1324167744
-rwxrwxr-x     1 sophie  staff         5865 Jul 26  2018 ECHO_May2018_mapping.txt
-rwxrwxr-x     1 sophie  staff        16306 Jul 26  2018 ECHO_May2018_mapping.xlsx
-rwxrwxr-x     1 sophie  staff       261114 Jul 18  2018 February 2018 Illumina & Metagenome Sequencing.xlsx
-rwxrwxr-x     1 sophie  staff        35359 Jul 18  2018 IMR-SampleSubmissionSheet_v8(Rowland_Feb20).xlsx
-rwxrwxr-x     1 sophie  staff        38317 Jul 18  2018 IMR-SampleSubmissionSheet_v8(Rowland_May17).xlsx
-rwxrwxr-x     1 sophie  staff          354 May 30  2018 README.rtf
drwxrwxr-x   763 kevin   staff        25942 Jan 22  2019 RowlandMetaG
drwxr-xr-x     2 sophie  staff           68 Oct 22 13:56 RowlandNS44_part1
drwxr-xr-x     2 sophie  staff           68 Oct 22 13:56 RowlandNS44_part2
drwxr-xr-x     2 sophie  staff           68 Oct 22 13:56 RowlandNS44_part3
drwxr-xr-x     2 sophie  staff           68 Oct 22 13:56 RowlandNS44_part4
drwxr-xr-x     2 sophie  staff           68 Oct 22 13:56 RowlandNS44_part5
drwxr-xr-x     2 sophie  staff           68 Oct 22 13:56 RowlandNS44_part6
drwxrwxr-x     2 sophie  staff           68 Jun  4  2019 RowlandRunNS38_part1
drwxrwxr-x     2 sophie  staff           68 Jun  4  2019 RowlandRunNS38_part2
drwxrwxr-x     2 sophie  staff           68 Jun  4  2019 RowlandRunNS38_part3
drwxrwxr-x     2 sophie  staff           68 Jun  4  2019 RowlandRunNS38_part4
drwxrwxr-x     2 sophie  staff           68 Jun  4  2019 RowlandRunNS38_part5
drwxrwxr-x     2 sophie  staff           68 Jun  4  2019 RowlandRunNS38_part6
drwxrwxr-x     2 sophie  staff           68 Jun  4  2019 RowlandRunNS39-remainder
drwxr-xr-x     2 sophie  staff           68 Sep 10 12:03 RowlandRunNS42_part1
drwxr-xr-x     2 sophie  staff           68 Sep 10 12:03 RowlandRunNS42_part2
drwxr-xr-x     2 sophie  staff           68 Sep 10 12:03 RowlandRunNS42_part3
drwxr-xr-x     2 sophie  staff           68 Sep 10 12:03 RowlandRunNS42_part4
drwxr-xr-x     2 sophie  staff           68 Sep 10 12:03 RowlandRunNS42_part5
drwxr-xr-x     2 sophie  staff           68 Sep 10 12:03 RowlandRunNS42_part6
drwxr-xr-x     2 sophie  staff           68 Sep 18 17:57 RowlandRunNS43_part1
drwxr-xr-x     2 sophie  staff           68 Sep 18 17:57 RowlandRunNS43_part2
drwxr-xr-x     2 sophie  staff           68 Sep 18 17:57 RowlandRunNS43_part3
drwxr-xr-x     2 sophie  staff           68 Sep 18 17:58 RowlandRunNS43_part4
drwxr-xr-x     2 sophie  staff           68 Sep 18 17:58 RowlandRunNS43_part5
drwxr-xr-x     2 sophie  staff           68 Sep 18 17:58 RowlandRunNS43_part6
-rwxrwxr-x     1 kevin   staff  75492823670 Jan 21  2019 apr2018.zip
-rw-r--r--     1 sophie  staff  14698942886 Sep  9 13:57 aug2019_1.zip
-rw-r--r--     1 sophie  staff  15517466965 Sep  9 14:17 aug2019_2.zip
-rw-r--r--     1 sophie  staff  17692116315 Sep  9 14:06 aug2019_3.zip
-rw-r--r--     1 sophie  staff  14474183863 Sep  9 14:03 aug2019_4.zip
-rw-r--r--     1 sophie  staff  16998573493 Sep  9 14:13 aug2019_5.zip
-rw-r--r--     1 sophie  staff  15055872754 Sep  9 14:18 aug2019_6.zip
-rwxrwxr-x     1 kevin   staff        23608 Apr 20  2019 batch001_files.txt
-rwxrwxr-x     1 kevin   staff        28048 Apr 20  2019 batch002_files.txt
-rwxrwxr-x     1 kevin   staff        22424 Apr 20  2019 batch003_files.txt
-rwxrwxr-x     1 kevin   staff        17992 Apr 23  2019 batch004_files.txt
-rwxrwxr-x     1 kevin   staff        28344 Apr 18  2019 batch005_files.txt
-rwxrwxr-x     1 kevin   staff        19464 Jan 19  2019 batch006_files.txt
-rwxrwxr-x     1 sophie  staff        19168 Mar 12  2019 batch007_files.txt
-rwxrwxr-x     1 sophie  staff        28344 Apr  5  2019 batch008_files.txt
-rwxrwxr-x     1 sophie  staff        38336 Jun  4  2019 batch009_files.txt
drwxr-xr-x    15 kevin   staff          510 Aug 30 16:07 batch010
-rw-r--r--     1 sophie  staff        28344 Sep 10 09:32 batch010_files.txt
-rw-r--r--     1 sophie  staff        28344 Sep 18 17:11 batch011_files.txt
-rw-r--r--     1 sophie  staff        28352 Oct 22 10:51 batch012_files.txt
-rwxrwxr-x     1 sophie  staff  25085043521 Mar 12  2019 dec2018_1.zip
-rwxrwxr-x     1 sophie  staff  23871427138 Mar 12  2019 dec2018_2.zip
-rwxrwxr-x     1 sophie  staff  24985890515 Mar 12  2019 dec2018_3.zip
drwxrwxr-x    73 kevin   staff         2482 Oct 22 12:40 details
-rwxrwxr-x     1 sophie  staff  12818025980 Apr  4  2019 feb2019_1.zip
-rwxrwxr-x     1 sophie  staff  13020378264 Apr  4  2019 feb2019_2.zip
-rwxrwxr-x     1 sophie  staff  15442389636 Apr  4  2019 feb2019_3.zip
-rwxrwxr-x     1 sophie  staff  15000386980 Apr  4  2019 feb2019_4.zip
-rwxrwxr-x     1 sophie  staff  13670082991 Apr  4  2019 feb2019_5.zip
-rwxrwxr-x     1 sophie  staff  14591216832 Apr  4  2019 feb2019_6.zip
-rwxrwxr-x     1 sophie  staff  14609065300 Jun  4  2019 may2019_1.zip
-rwxrwxr-x     1 sophie  staff  12369427841 Jun  4  2019 may2019_2.zip
-rwxrwxr-x     1 sophie  staff  14144220803 Jun  4  2019 may2019_3.zip
-rwxrwxr-x     1 sophie  staff  15537372869 Jun  4  2019 may2019_4.zip
-rwxrwxr-x     1 sophie  staff  13055782071 Jun  4  2019 may2019_5.zip
-rwxrwxr-x     1 sophie  staff  13340576337 Jun  4  2019 may2019_6.zip
-rwxrwxr-x     1 sophie  staff  35844182939 Jun  4  2019 may2019_7.zip
-rwxrwxr-x     1 kevin   staff          296 Apr 23  2019 missing.txt
-rwxrwxr-x     1 kevin   staff         1184 Apr 23  2019 nobatch.txt
-rwxrwxr-x     1 kevin   staff  77388259227 Jan 18  2019 nov2018.zip
-rw-r--r--     1 sophie  staff  12620011783 Oct 21 14:48 oct2019_1.zip
-rw-r--r--     1 sophie  staff  13081509700 Oct 21 14:55 oct2019_2.zip
-rw-r--r--     1 sophie  staff  11247767313 Oct 21 14:56 oct2019_3.zip
-rw-r--r--     1 sophie  staff  12018232158 Oct 21 14:57 oct2019_4.zip
-rw-r--r--     1 sophie  staff  12199788900 Oct 21 14:57 oct2019_5.zip
-rw-r--r--     1 sophie  staff  13383668214 Oct 21 14:57 oct2019_6.zip
-rw-r--r--     1 sophie  staff         4696 Sep 10 11:58 p1.txt
drwxrwxr-x  8450 kevin   staff       287300 Oct 22 13:56 rawfastq
-rwxrwxr-x     1 kevin   staff        68280 Jan 28  2019 rawfastq_batch005.txt
-rw-r--r--     1 sophie  staff  13950521134 Sep 18 11:23 sep2019_1.zip
-rw-r--r--     1 sophie  staff  11916437699 Sep 18 11:22 sep2019_2.zip
-rw-r--r--     1 sophie  staff  13319331835 Sep 18 11:28 sep2019_3.zip
-rw-r--r--     1 sophie  staff  12688859783 Sep 18 11:25 sep2019_4.zip
-rw-r--r--     1 sophie  staff  12913826643 Sep 18 11:25 sep2019_5.zip
-rw-r--r--     1 sophie  staff  13929224254 Sep 18 11:26 sep2019_6.zip
drwx------    12 sophie  staff          408 Aug 28 12:27 submissions
-rwxrwxr-x     1 sophie  staff          165 Jul 27  2018 ~$IMR-SampleSubmissionSheet_v8(Rowland_May17).xlsx
$ ls -l sequencing/
total 0
drwx------  21 sophie  staff   714 Jul 18 11:24 16S
drwxrwxr-x  92 sophie  staff  3128 Oct 22 10:49 mgx
# /Volumes/vkclab/echo/
$ ls -l sequencing/
total 92063840
drwx------  1 sophie  staff        16384 Jul 18 11:24 16S
drwx------  1 sophie  staff        16384 Oct 22 10:49 mgx
drwx------  1 sophie  staff        16384 Jan 28  2019 mgx_old
-rwx------  1 sophie  staff  47136636928 Jan 22  2019 mgx_old.tar.gz
$ ls -l sequencing/mgx
total 1324168672
-rwx------  1 sophie  staff         5865 Jul 26  2018 ECHO_May2018_mapping.txt
-rwx------  1 sophie  staff        16306 Jul 26  2018 ECHO_May2018_mapping.xlsx
-rwx------  1 sophie  staff       261114 Jul 18  2018 February 2018 Illumina & Metagenome Sequencing.xlsx
-rwx------  1 sophie  staff        35359 Jul 18  2018 IMR-SampleSubmissionSheet_v8(Rowland_Feb20).xlsx
-rwx------  1 sophie  staff        38317 Jul 18  2018 IMR-SampleSubmissionSheet_v8(Rowland_May17).xlsx
-rwx------  1 sophie  staff          354 May 30  2018 README.rtf
drwx------  1 sophie  staff        16384 Jan 22  2019 RowlandMetaG
drwx------  1 sophie  staff        16384 Oct 22 13:56 RowlandNS44_part1
drwx------  1 sophie  staff        16384 Oct 22 13:56 RowlandNS44_part2
drwx------  1 sophie  staff        16384 Oct 22 13:56 RowlandNS44_part3
drwx------  1 sophie  staff        16384 Oct 22 13:56 RowlandNS44_part4
drwx------  1 sophie  staff        16384 Oct 22 13:56 RowlandNS44_part5
drwx------  1 sophie  staff        16384 Oct 22 13:56 RowlandNS44_part6
drwx------  1 sophie  staff        16384 Jun  4  2019 RowlandRunNS38_part1
drwx------  1 sophie  staff        16384 Jun  4  2019 RowlandRunNS38_part2
drwx------  1 sophie  staff        16384 Jun  4  2019 RowlandRunNS38_part3
drwx------  1 sophie  staff        16384 Jun  4  2019 RowlandRunNS38_part4
drwx------  1 sophie  staff        16384 Jun  4  2019 RowlandRunNS38_part5
drwx------  1 sophie  staff        16384 Jun  4  2019 RowlandRunNS38_part6
drwx------  1 sophie  staff        16384 Jun  4  2019 RowlandRunNS39-remainder
drwx------  1 sophie  staff        16384 Sep 10 12:03 RowlandRunNS42_part1
drwx------  1 sophie  staff        16384 Sep 10 12:03 RowlandRunNS42_part2
drwx------  1 sophie  staff        16384 Sep 10 12:03 RowlandRunNS42_part3
drwx------  1 sophie  staff        16384 Sep 10 12:03 RowlandRunNS42_part4
drwx------  1 sophie  staff        16384 Sep 10 12:03 RowlandRunNS42_part5
drwx------  1 sophie  staff        16384 Sep 10 12:03 RowlandRunNS42_part6
drwx------  1 sophie  staff        16384 Sep 18 17:57 RowlandRunNS43_part1
drwx------  1 sophie  staff        16384 Sep 18 17:57 RowlandRunNS43_part2
drwx------  1 sophie  staff        16384 Sep 18 17:57 RowlandRunNS43_part3
drwx------  1 sophie  staff        16384 Sep 18 17:58 RowlandRunNS43_part4
drwx------  1 sophie  staff        16384 Sep 18 17:58 RowlandRunNS43_part5
drwx------  1 sophie  staff        16384 Sep 18 17:58 RowlandRunNS43_part6
-rwx------  1 sophie  staff  75492823670 Jan 21  2019 apr2018.zip
-rwx------  1 sophie  staff  14698942886 Sep  9 13:57 aug2019_1.zip
-rwx------  1 sophie  staff  15517466965 Sep  9 14:17 aug2019_2.zip
-rwx------  1 sophie  staff  17692116315 Sep  9 14:06 aug2019_3.zip
-rwx------  1 sophie  staff  14474183863 Sep  9 14:03 aug2019_4.zip
-rwx------  1 sophie  staff  16998573493 Sep  9 14:13 aug2019_5.zip
-rwx------  1 sophie  staff  15055872754 Sep  9 14:18 aug2019_6.zip
-rwx------  1 sophie  staff        23608 Apr 20  2019 batch001_files.txt
-rwx------  1 sophie  staff       119632 Jan 22  2019 batch002-3_files.txt
-rwx------  1 sophie  staff        28048 Apr 20  2019 batch002_files.txt
-rwx------  1 sophie  staff        22424 Apr 20  2019 batch003_files.txt
-rwx------  1 sophie  staff        17992 Apr 23  2019 batch004_files.txt
-rwx------  1 sophie  staff         1249 Jan 28  2019 batch005_airtable.txt
-rwx------  1 sophie  staff         1250 Jan 28  2019 batch005_airtable_fix.txt
-rwx------  1 sophie  staff        28344 Apr 18  2019 batch005_files.txt
-rwx------  1 sophie  staff        19464 Jan 19  2019 batch006_files.txt
-rwx------  1 sophie  staff        19168 Mar 12  2019 batch007_files.txt
-rwx------  1 sophie  staff        28344 Apr  5  2019 batch008_files.txt
-rwx------  1 sophie  staff        38336 Jun  4  2019 batch009_files.txt
drwx------  1 sophie  staff        16384 Aug 30 16:07 batch010
-rwx------  1 sophie  staff        28344 Sep 10 09:32 batch010_files.txt
-rwx------  1 sophie  staff        28344 Sep 18 17:11 batch011_files.txt
-rwx------  1 sophie  staff        28352 Oct 22 10:51 batch012_files.txt
-rwx------  1 sophie  staff  25085043521 Mar 12  2019 dec2018_1.zip
-rwx------  1 sophie  staff  23871427138 Mar 12  2019 dec2018_2.zip
-rwx------  1 sophie  staff  24985890515 Mar 12  2019 dec2018_3.zip
drwx------  1 sophie  staff        16384 Oct 22 12:40 details
-rwx------  1 sophie  staff  12818025980 Apr  4  2019 feb2019_1.zip
-rwx------  1 sophie  staff  13020378264 Apr  4  2019 feb2019_2.zip
-rwx------  1 sophie  staff  15442389636 Apr  4  2019 feb2019_3.zip
-rwx------  1 sophie  staff  15000386980 Apr  4  2019 feb2019_4.zip
-rwx------  1 sophie  staff  13670082991 Apr  4  2019 feb2019_5.zip
-rwx------  1 sophie  staff  14591216832 Apr  4  2019 feb2019_6.zip
-rwx------  1 sophie  staff  14609065300 Jun  4  2019 may2019_1.zip
-rwx------  1 sophie  staff  12369427841 Jun  4  2019 may2019_2.zip
-rwx------  1 sophie  staff  14144220803 Jun  4  2019 may2019_3.zip
-rwx------  1 sophie  staff  15537372869 Jun  4  2019 may2019_4.zip
-rwx------  1 sophie  staff  13055782071 Jun  4  2019 may2019_5.zip
-rwx------  1 sophie  staff  13340576337 Jun  4  2019 may2019_6.zip
-rwx------  1 sophie  staff  35844182939 Jun  4  2019 may2019_7.zip
-rwx------  1 sophie  staff          296 Apr 23  2019 missing.txt
-rwx------  1 sophie  staff         1184 Apr 23  2019 nobatch.txt
-rwx------  1 sophie  staff  77388259227 Jan 18  2019 nov2018.zip
-rwx------  1 sophie  staff  12620011783 Oct 21 14:48 oct2019_1.zip
-rwx------  1 sophie  staff  13081509700 Oct 21 14:55 oct2019_2.zip
-rwx------  1 sophie  staff  11247767313 Oct 21 14:56 oct2019_3.zip
-rwx------  1 sophie  staff  12018232158 Oct 21 14:57 oct2019_4.zip
-rwx------  1 sophie  staff  12199788900 Oct 21 14:57 oct2019_5.zip
-rwx------  1 sophie  staff  13383668214 Oct 21 14:57 oct2019_6.zip
-rwx------  1 sophie  staff         4696 Sep 10 11:58 p1.txt
drwx------  1 sophie  staff        16384 Oct 22 13:56 rawfastq
-rwx------  1 sophie  staff        68280 Jan 28  2019 rawfastq_batch005.txt
-rwx------  1 sophie  staff  13950521134 Sep 18 11:23 sep2019_1.zip
-rwx------  1 sophie  staff  11916437699 Sep 18 11:22 sep2019_2.zip
-rwx------  1 sophie  staff  13319331835 Sep 18 11:28 sep2019_3.zip
-rwx------  1 sophie  staff  12688859783 Sep 18 11:25 sep2019_4.zip
-rwx------  1 sophie  staff  12913826643 Sep 18 11:25 sep2019_5.zip
-rwx------  1 sophie  staff  13929224254 Sep 18 11:26 sep2019_6.zip
drwx------  1 sophie  staff        16384 Aug 28 12:27 submissions
-rwx------  1 sophie  staff          165 Jul 27  2018 ~$IMR-SampleSubmissionSheet_v8(Rowland_May17).xlsx
```

## Cleaning /lovelace/echo/sequencing/mgx/

```sh
$ ssh ada
$ cd /lovelace/echo/sequencing/mgx/
$ ls -l
total 697088792
-rwxrwxr-x 1 sophie sophie           165 Jul 27  2018 '~$IMR-SampleSubmissionSheet_v8(Rowland_May17).xlsx'
-rwxrwxr-x 1 sophie sophie   75492823670 Jan 21  2019  apr2018.zip
-rw-r--r-- 1 sophie facstaff 14698942886 Sep  9 13:57  aug2019_1.zip
-rw-r--r-- 1 sophie facstaff 15517466965 Sep  9 14:17  aug2019_2.zip
-rw-r--r-- 1 sophie facstaff 17692116315 Sep  9 14:06  aug2019_3.zip
-rw-r--r-- 1 sophie facstaff 14474183863 Sep  9 14:03  aug2019_4.zip
-rw-r--r-- 1 sophie facstaff 16998573493 Sep  9 14:13  aug2019_5.zip
-rw-r--r-- 1 sophie facstaff 15055872754 Sep  9 14:18  aug2019_6.zip
-rwxrwxr-x 1 sophie sophie         23608 Apr 20  2019  batch001_files.txt
-rwxrwxr-x 1 sophie sophie         28048 Apr 20  2019  batch002_files.txt
-rwxrwxr-x 1 sophie sophie         22424 Apr 20  2019  batch003_files.txt
-rwxrwxr-x 1 sophie sophie         17992 Apr 23  2019  batch004_files.txt
-rwxrwxr-x 1 sophie sophie         28344 Apr 18  2019  batch005_files.txt
-rwxrwxr-x 1 sophie sophie         19464 Jan 19  2019  batch006_files.txt
-rwxrwxr-x 1 sophie sophie         19168 Mar 12  2019  batch007_files.txt
-rwxrwxr-x 1 sophie sophie         28344 Apr  5  2019  batch008_files.txt
drwxrwxr-x 4 kevin  kevin           4096 May 28 10:15  batch009
-rw-rw-r-- 1 root   root     35844182939 May 30 19:02  batch009.2.zip
-rwxrwxr-x 1 sophie sophie         38336 Jun  4 13:29  batch009_files.txt
drwxrwxr-x 8 kevin  facstaff        4096 Sep 10 13:22  batch010
-rw-r--r-- 1 sophie facstaff       28344 Sep 10 09:32  batch010_files.txt
-rw-r--r-- 1 sophie facstaff       28344 Sep 18 17:11  batch011_files.txt
-rw-r--r-- 1 sophie facstaff       28352 Oct 22 10:51  batch012_files.txt
-rwxrwxr-x 1 sophie sophie   25085043521 Mar 12  2019  dec2018_1.zip
-rwxrwxr-x 1 sophie sophie   23871427138 Mar 12  2019  dec2018_2.zip
-rwxrwxr-x 1 sophie sophie   24985890515 Mar 12  2019  dec2018_3.zip
drwxrwxr-x 2 sophie sophie          4096 Oct 22 12:40  details
-rwxrwxr-x 1 sophie sophie          5865 Jul 26  2018  ECHO_May2018_mapping.txt
-rwxrwxr-x 1 sophie sophie         16306 Jul 26  2018  ECHO_May2018_mapping.xlsx
-rwxrwxr-x 1 sophie sophie   12818025980 Apr  4  2019  feb2019_1.zip
-rwxrwxr-x 1 sophie sophie   13020378264 Apr  4  2019  feb2019_2.zip
-rwxrwxr-x 1 sophie sophie   15442389636 Apr  4  2019  feb2019_3.zip
-rwxrwxr-x 1 sophie sophie   15000386980 Apr  4  2019  feb2019_4.zip
-rwxrwxr-x 1 sophie sophie   13670082991 Apr  4  2019  feb2019_5.zip
-rwxrwxr-x 1 sophie sophie   14591216832 Apr  4  2019  feb2019_6.zip
-rwxrwxr-x 1 sophie sophie        261114 Jul 18  2018 'February 2018 Illumina & Metagenome Sequencing.xlsx'
-rwxrwxr-x 1 sophie sophie         35359 Jul 18  2018 'IMR-SampleSubmissionSheet_v8(Rowland_Feb20).xlsx'
-rwxrwxr-x 1 sophie sophie         38317 Jul 18  2018 'IMR-SampleSubmissionSheet_v8(Rowland_May17).xlsx'
-rwxrwxr-x 1 sophie sophie   14609065300 Jun  4 09:51  may2019_1.zip
-rwxrwxr-x 1 sophie sophie   12369427841 Jun  4 10:00  may2019_2.zip
-rwxrwxr-x 1 sophie sophie   14144220803 Jun  4 10:01  may2019_3.zip
-rwxrwxr-x 1 sophie sophie   15537372869 Jun  4 10:03  may2019_4.zip
-rwxrwxr-x 1 sophie sophie   13055782071 Jun  4 10:02  may2019_5.zip
-rwxrwxr-x 1 sophie sophie   13340576337 Jun  4 10:30  may2019_6.zip
-rwxrwxr-x 1 sophie sophie   35844182939 Jun  4 10:40  may2019_7.zip
-rwxrwxr-x 1 sophie sophie           296 Apr 23  2019  missing.txt
-rw-rw-r-- 1 kevin  kevin          71624 Aug 13 10:10  mothers_fastq.txt
-rwxrwxr-x 1 sophie sophie          1184 Apr 23  2019  nobatch.txt
-rwxrwxr-x 1 sophie sophie   77388259227 Jan 18  2019  nov2018.zip
-rw-r--r-- 1 sophie facstaff 12620011783 Oct 21 14:48  oct2019_1.zip
-rw-r--r-- 1 sophie facstaff 13081509700 Oct 21 14:55  oct2019_2.zip
-rw-r--r-- 1 sophie facstaff 11247767313 Oct 21 14:56  oct2019_3.zip
-rw-r--r-- 1 sophie facstaff 12018232158 Oct 21 14:57  oct2019_4.zip
-rw-r--r-- 1 sophie facstaff 12199788900 Oct 21 14:57  oct2019_5.zip
-rw-r--r-- 1 sophie facstaff 13383668214 Oct 21 14:57  oct2019_6.zip
-rw-r--r-- 1 sophie facstaff        4696 Sep 10 11:58  p1.txt
drwxrwxr-x 2 sophie sophie        536576 Oct 22 13:56  rawfastq
-rwxrwxr-x 1 sophie sophie         68280 Jan 28  2019  rawfastq_batch005.txt
-rwxrwxr-x 1 sophie sophie           354 May 30  2018  README.rtf
drwxrwxr-x 3 sophie sophie         57344 Jan 22  2019  RowlandMetaG
drwxr-xr-x 2 sophie facstaff        4096 Oct 22 13:56  RowlandNS44_part1
drwxr-xr-x 2 sophie facstaff        4096 Oct 22 13:56  RowlandNS44_part2
drwxr-xr-x 2 sophie facstaff        4096 Oct 22 13:56  RowlandNS44_part3
drwxr-xr-x 2 sophie facstaff        4096 Oct 22 13:56  RowlandNS44_part4
drwxr-xr-x 2 sophie facstaff        4096 Oct 22 13:56  RowlandNS44_part5
drwxr-xr-x 2 sophie facstaff        4096 Oct 22 13:56  RowlandNS44_part6
drwxrwxr-x 2 sophie sophie          4096 Jun  4 14:19  RowlandRunNS38_part1
drwxrwxr-x 2 sophie sophie          4096 Jun  4 14:19  RowlandRunNS38_part2
drwxrwxr-x 2 sophie sophie          4096 Jun  4 14:19  RowlandRunNS38_part3
drwxrwxr-x 2 sophie sophie          4096 Jun  4 14:19  RowlandRunNS38_part4
drwxrwxr-x 2 sophie sophie          4096 Jun  4 14:19  RowlandRunNS38_part5
drwxrwxr-x 2 sophie sophie          4096 Jun  4 14:20  RowlandRunNS38_part6
drwxr-xr-x 3 root   root           20480 May 30 16:23  RowlandRunNS39-remainder
drwxr-xr-x 2 sophie facstaff        4096 Sep 10 12:03  RowlandRunNS42_part1
drwxr-xr-x 2 sophie facstaff        4096 Sep 10 12:03  RowlandRunNS42_part2
drwxr-xr-x 2 sophie facstaff        4096 Sep 10 12:03  RowlandRunNS42_part3
drwxr-xr-x 2 sophie facstaff        4096 Sep 10 12:03  RowlandRunNS42_part4
drwxr-xr-x 2 sophie facstaff        4096 Sep 10 12:03  RowlandRunNS42_part5
drwxr-xr-x 2 sophie facstaff        4096 Sep 10 12:03  RowlandRunNS42_part6
drwxr-xr-x 2 sophie facstaff        4096 Sep 18 17:57  RowlandRunNS43_part1
drwxr-xr-x 2 sophie facstaff        4096 Sep 18 17:57  RowlandRunNS43_part2
drwxr-xr-x 2 sophie facstaff        4096 Sep 18 17:57  RowlandRunNS43_part3
drwxr-xr-x 2 sophie facstaff        4096 Sep 18 17:58  RowlandRunNS43_part4
drwxr-xr-x 2 sophie facstaff        4096 Sep 18 17:58  RowlandRunNS43_part5
drwxr-xr-x 2 sophie facstaff        4096 Sep 18 17:58  RowlandRunNS43_part6
-rw-r--r-- 1 sophie facstaff 13950521134 Sep 18 11:23  sep2019_1.zip
-rw-r--r-- 1 sophie facstaff 11916437699 Sep 18 11:22  sep2019_2.zip
-rw-r--r-- 1 sophie facstaff 13319331835 Sep 18 11:28  sep2019_3.zip
-rw-r--r-- 1 sophie facstaff 12688859783 Sep 18 11:25  sep2019_4.zip
-rw-r--r-- 1 sophie facstaff 12913826643 Sep 18 11:25  sep2019_5.zip
-rw-r--r-- 1 sophie facstaff 13929224254 Sep 18 11:26  sep2019_6.zip
drwx------ 2 sophie facstaff        4096 Aug 28 12:27  submissions
```

Many redundant things. Let's start with the `.zip` files.

```sh
$ ls -l *.zip
-rwxrwxr-x 1 sophie sophie   75492823670 Jan 21  2019 apr2018.zip
-rw-r--r-- 1 sophie facstaff 14698942886 Sep  9 13:57 aug2019_1.zip
-rw-r--r-- 1 sophie facstaff 15517466965 Sep  9 14:17 aug2019_2.zip
-rw-r--r-- 1 sophie facstaff 17692116315 Sep  9 14:06 aug2019_3.zip
-rw-r--r-- 1 sophie facstaff 14474183863 Sep  9 14:03 aug2019_4.zip
-rw-r--r-- 1 sophie facstaff 16998573493 Sep  9 14:13 aug2019_5.zip
-rw-r--r-- 1 sophie facstaff 15055872754 Sep  9 14:18 aug2019_6.zip
-rw-rw-r-- 1 root   root     35844182939 May 30 19:02 batch009.2.zip
-rwxrwxr-x 1 sophie sophie   25085043521 Mar 12  2019 dec2018_1.zip
-rwxrwxr-x 1 sophie sophie   23871427138 Mar 12  2019 dec2018_2.zip
-rwxrwxr-x 1 sophie sophie   24985890515 Mar 12  2019 dec2018_3.zip
-rwxrwxr-x 1 sophie sophie   12818025980 Apr  4  2019 feb2019_1.zip
-rwxrwxr-x 1 sophie sophie   13020378264 Apr  4  2019 feb2019_2.zip
-rwxrwxr-x 1 sophie sophie   15442389636 Apr  4  2019 feb2019_3.zip
-rwxrwxr-x 1 sophie sophie   15000386980 Apr  4  2019 feb2019_4.zip
-rwxrwxr-x 1 sophie sophie   13670082991 Apr  4  2019 feb2019_5.zip
-rwxrwxr-x 1 sophie sophie   14591216832 Apr  4  2019 feb2019_6.zip
-rwxrwxr-x 1 sophie sophie   14609065300 Jun  4 09:51 may2019_1.zip
-rwxrwxr-x 1 sophie sophie   12369427841 Jun  4 10:00 may2019_2.zip
-rwxrwxr-x 1 sophie sophie   14144220803 Jun  4 10:01 may2019_3.zip
-rwxrwxr-x 1 sophie sophie   15537372869 Jun  4 10:03 may2019_4.zip
-rwxrwxr-x 1 sophie sophie   13055782071 Jun  4 10:02 may2019_5.zip
-rwxrwxr-x 1 sophie sophie   13340576337 Jun  4 10:30 may2019_6.zip
-rwxrwxr-x 1 sophie sophie   35844182939 Jun  4 10:40 may2019_7.zip
-rwxrwxr-x 1 sophie sophie   77388259227 Jan 18  2019 nov2018.zip
-rw-r--r-- 1 sophie facstaff 12620011783 Oct 21 14:48 oct2019_1.zip
-rw-r--r-- 1 sophie facstaff 13081509700 Oct 21 14:55 oct2019_2.zip
-rw-r--r-- 1 sophie facstaff 11247767313 Oct 21 14:56 oct2019_3.zip
-rw-r--r-- 1 sophie facstaff 12018232158 Oct 21 14:57 oct2019_4.zip
-rw-r--r-- 1 sophie facstaff 12199788900 Oct 21 14:57 oct2019_5.zip
-rw-r--r-- 1 sophie facstaff 13383668214 Oct 21 14:57 oct2019_6.zip
-rw-r--r-- 1 sophie facstaff 13950521134 Sep 18 11:23 sep2019_1.zip
-rw-r--r-- 1 sophie facstaff 11916437699 Sep 18 11:22 sep2019_2.zip
-rw-r--r-- 1 sophie facstaff 13319331835 Sep 18 11:28 sep2019_3.zip
-rw-r--r-- 1 sophie facstaff 12688859783 Sep 18 11:25 sep2019_4.zip
-rw-r--r-- 1 sophie facstaff 12913826643 Sep 18 11:25 sep2019_5.zip
-rw-r--r-- 1 sophie facstaff 13929224254 Sep 18 11:26 sep2019_6.zip
```

I will go systematically by `MMMYYYY_#.zip`, which should correspond to specific batch###'s.

```sh
# batch012
$ ls -l oct*
-rw-r--r-- 1 sophie facstaff 12620011783 Oct 21 14:48 oct2019_1.zip
-rw-r--r-- 1 sophie facstaff 13081509700 Oct 21 14:55 oct2019_2.zip
-rw-r--r-- 1 sophie facstaff 11247767313 Oct 21 14:56 oct2019_3.zip
-rw-r--r-- 1 sophie facstaff 12018232158 Oct 21 14:57 oct2019_4.zip
-rw-r--r-- 1 sophie facstaff 12199788900 Oct 21 14:57 oct2019_5.zip
-rw-r--r-- 1 sophie facstaff 13383668214 Oct 21 14:57 oct2019_6.zip
$ rm oct*

# batch011
$ ls -l sep*
-rw-r--r-- 1 sophie facstaff 13950521134 Sep 18 11:23 sep2019_1.zip
-rw-r--r-- 1 sophie facstaff 11916437699 Sep 18 11:22 sep2019_2.zip
-rw-r--r-- 1 sophie facstaff 13319331835 Sep 18 11:28 sep2019_3.zip
-rw-r--r-- 1 sophie facstaff 12688859783 Sep 18 11:25 sep2019_4.zip
-rw-r--r-- 1 sophie facstaff 12913826643 Sep 18 11:25 sep2019_5.zip
-rw-r--r-- 1 sophie facstaff 13929224254 Sep 18 11:26 sep2019_6.zip
$ rm sep*

# batch010
$ ls -l aug*
-rw-r--r-- 1 sophie facstaff 14698942886 Sep  9 13:57 aug2019_1.zip
-rw-r--r-- 1 sophie facstaff 15517466965 Sep  9 14:17 aug2019_2.zip
-rw-r--r-- 1 sophie facstaff 17692116315 Sep  9 14:06 aug2019_3.zip
-rw-r--r-- 1 sophie facstaff 14474183863 Sep  9 14:03 aug2019_4.zip
-rw-r--r-- 1 sophie facstaff 16998573493 Sep  9 14:13 aug2019_5.zip
-rw-r--r-- 1 sophie facstaff 15055872754 Sep  9 14:18 aug2019_6.zip
$ rm aug*

# batch009
$ ls -l may*
-rwxrwxr-x 1 sophie sophie 14609065300 Jun  4 09:51 may2019_1.zip
-rwxrwxr-x 1 sophie sophie 12369427841 Jun  4 10:00 may2019_2.zip
-rwxrwxr-x 1 sophie sophie 14144220803 Jun  4 10:01 may2019_3.zip
-rwxrwxr-x 1 sophie sophie 15537372869 Jun  4 10:03 may2019_4.zip
-rwxrwxr-x 1 sophie sophie 13055782071 Jun  4 10:02 may2019_5.zip
-rwxrwxr-x 1 sophie sophie 13340576337 Jun  4 10:30 may2019_6.zip
-rwxrwxr-x 1 sophie sophie 35844182939 Jun  4 10:40 may2019_7.zip
$ rm may*
$ rm batch009.2.zip

# batch008
$ ls -l feb*
-rwxrwxr-x 1 sophie sophie 12818025980 Apr  4  2019 feb2019_1.zip
-rwxrwxr-x 1 sophie sophie 13020378264 Apr  4  2019 feb2019_2.zip
-rwxrwxr-x 1 sophie sophie 15442389636 Apr  4  2019 feb2019_3.zip
-rwxrwxr-x 1 sophie sophie 15000386980 Apr  4  2019 feb2019_4.zip
-rwxrwxr-x 1 sophie sophie 13670082991 Apr  4  2019 feb2019_5.zip
-rwxrwxr-x 1 sophie sophie 14591216832 Apr  4  2019 feb2019_6.zip
$ rm feb*

# batch007
$ ls -l dec*
-rwxrwxr-x 1 sophie sophie 25085043521 Mar 12  2019 dec2018_1.zip
-rwxrwxr-x 1 sophie sophie 23871427138 Mar 12  2019 dec2018_2.zip
-rwxrwxr-x 1 sophie sophie 24985890515 Mar 12  2019 dec2018_3.zip
$ rm dec*

# remaining zip files
$ ls -l *.zip
-rwxrwxr-x 1 sophie sophie 75492823670 Jan 21  2019 apr2018.zip
-rw-rw-r-- 1 root   root   35844182939 May 30 19:02 batch009.2.zip
-rwxrwxr-x 1 sophie sophie 77388259227 Jan 18  2019 nov2018.zip
# check batch009 files are all present in rawfastq/
$ ls rawfastq/ | grep --file=batch009_files.txt | wc
   1040    1040   38336
$ wc batch009_files.txt
 1040  1040 38336 batch009_files.txt
$ rm *.zip
```

Now onto the (supposedly) empty unzipped folders.

```sh
$ ls -l
...
drwxrwxr-x 3 sophie sophie    57344 Jan 22  2019  RowlandMetaG
drwxr-xr-x 2 sophie facstaff   4096 Oct 22 13:56  RowlandNS44_part1
drwxr-xr-x 2 sophie facstaff   4096 Oct 22 13:56  RowlandNS44_part2
drwxr-xr-x 2 sophie facstaff   4096 Oct 22 13:56  RowlandNS44_part3
drwxr-xr-x 2 sophie facstaff   4096 Oct 22 13:56  RowlandNS44_part4
drwxr-xr-x 2 sophie facstaff   4096 Oct 22 13:56  RowlandNS44_part5
drwxr-xr-x 2 sophie facstaff   4096 Oct 22 13:56  RowlandNS44_part6
drwxrwxr-x 2 sophie sophie     4096 Jun  4 14:19  RowlandRunNS38_part1
drwxrwxr-x 2 sophie sophie     4096 Jun  4 14:19  RowlandRunNS38_part2
drwxrwxr-x 2 sophie sophie     4096 Jun  4 14:19  RowlandRunNS38_part3
drwxrwxr-x 2 sophie sophie     4096 Jun  4 14:19  RowlandRunNS38_part4
drwxrwxr-x 2 sophie sophie     4096 Jun  4 14:19  RowlandRunNS38_part5
drwxrwxr-x 2 sophie sophie     4096 Jun  4 14:20  RowlandRunNS38_part6
drwxr-xr-x 3 root   root      20480 May 30 16:23  RowlandRunNS39-remainder
drwxr-xr-x 2 sophie facstaff   4096 Sep 10 12:03  RowlandRunNS42_part1
drwxr-xr-x 2 sophie facstaff   4096 Sep 10 12:03  RowlandRunNS42_part2
drwxr-xr-x 2 sophie facstaff   4096 Sep 10 12:03  RowlandRunNS42_part3
drwxr-xr-x 2 sophie facstaff   4096 Sep 10 12:03  RowlandRunNS42_part4
drwxr-xr-x 2 sophie facstaff   4096 Sep 10 12:03  RowlandRunNS42_part5
drwxr-xr-x 2 sophie facstaff   4096 Sep 10 12:03  RowlandRunNS42_part6
drwxr-xr-x 2 sophie facstaff   4096 Sep 18 17:57  RowlandRunNS43_part1
drwxr-xr-x 2 sophie facstaff   4096 Sep 18 17:57  RowlandRunNS43_part2
drwxr-xr-x 2 sophie facstaff   4096 Sep 18 17:57  RowlandRunNS43_part3
drwxr-xr-x 2 sophie facstaff   4096 Sep 18 17:58  RowlandRunNS43_part4
drwxr-xr-x 2 sophie facstaff   4096 Sep 18 17:58  RowlandRunNS43_part5
drwxr-xr-x 2 sophie facstaff   4096 Sep 18 17:58  RowlandRunNS43_part6
...

# Remove all above folders except RowlandRunNS39-remainder/ and RowlandMetaG/
$ ls RowlandRunNS4*
ls RowlandRunNS4*
RowlandRunNS42_part1:

RowlandRunNS42_part2:

RowlandRunNS42_part3:

RowlandRunNS42_part4:

RowlandRunNS42_part5:

RowlandRunNS42_part6:

RowlandRunNS43_part1:

RowlandRunNS43_part2:

RowlandRunNS43_part3:

RowlandRunNS43_part4:

RowlandRunNS43_part5:

RowlandRunNS43_part6:
$ rmdir RowlandRunNS4*
$ rmdir Rowland*
rmdir: failed to remove 'RowlandMetaG': Directory not empty
rmdir: failed to remove 'RowlandRunNS39-remainder': Directory not empty
$ ls Rowland*
```

What still remains? What can be removed? (Oct 25 2019)

```sh
$ ls -l
total 1456
-rwxrwxr-x 1 sophie sophie      165 Jul 27  2018 '~$IMR-SampleSubmissionSheet_v8(Rowland_May17).xlsx'
-rwxrwxr-x 1 sophie sophie    23608 Apr 20  2019  batch001_files.txt
-rwxrwxr-x 1 sophie sophie    28048 Apr 20  2019  batch002_files.txt
-rwxrwxr-x 1 sophie sophie    22424 Apr 20  2019  batch003_files.txt
-rwxrwxr-x 1 sophie sophie    17992 Apr 23  2019  batch004_files.txt
-rwxrwxr-x 1 sophie sophie    28344 Apr 18  2019  batch005_files.txt
-rwxrwxr-x 1 sophie sophie    19464 Jan 19  2019  batch006_files.txt
-rwxrwxr-x 1 sophie sophie    19168 Mar 12  2019  batch007_files.txt
-rwxrwxr-x 1 sophie sophie    28344 Apr  5  2019  batch008_files.txt
drwxrwxr-x 4 kevin  kevin      4096 May 28 10:15  batch009
-rwxrwxr-x 1 sophie sophie    38336 Jun  4 13:29  batch009_files.txt
drwxrwxr-x 8 kevin  facstaff   4096 Sep 10 13:22  batch010
-rw-r--r-- 1 sophie facstaff  28344 Sep 10 09:32  batch010_files.txt
-rw-r--r-- 1 sophie facstaff  28344 Sep 18 17:11  batch011_files.txt
-rw-r--r-- 1 sophie facstaff  28352 Oct 22 10:51  batch012_files.txt
drwxrwxr-x 2 sophie sophie     4096 Oct 22 12:40  details
-rwxrwxr-x 1 sophie sophie     5865 Jul 26  2018  ECHO_May2018_mapping.txt
-rwxrwxr-x 1 sophie sophie    16306 Jul 26  2018  ECHO_May2018_mapping.xlsx
-rwxrwxr-x 1 sophie sophie   261114 Jul 18  2018 'February 2018 Illumina & Metagenome Sequencing.xlsx'
-rwxrwxr-x 1 sophie sophie    35359 Jul 18  2018 'IMR-SampleSubmissionSheet_v8(Rowland_Feb20).xlsx'
-rwxrwxr-x 1 sophie sophie    38317 Jul 18  2018 'IMR-SampleSubmissionSheet_v8(Rowland_May17).xlsx'
-rwxrwxr-x 1 sophie sophie      296 Apr 23  2019  missing.txt
-rw-rw-r-- 1 kevin  kevin     71624 Aug 13 10:10  mothers_fastq.txt
-rwxrwxr-x 1 sophie sophie     1184 Apr 23  2019  nobatch.txt
-rw-r--r-- 1 sophie facstaff   4696 Sep 10 11:58  p1.txt
drwxrwxr-x 2 sophie sophie   536576 Oct 22 13:56  rawfastq
-rwxrwxr-x 1 sophie sophie    68280 Jan 28  2019  rawfastq_batch005.txt
-rwxrwxr-x 1 sophie sophie      354 May 30  2018  README.rtf
drwxrwxr-x 3 sophie sophie    57344 Jan 22  2019  RowlandMetaG
drwxr-xr-x 3 root   root      20480 May 30 16:23  RowlandRunNS39-remainder
drwx------ 2 sophie facstaff   4096 Aug 28 12:27  submissions
```

There are 4 folders with a lot of data that we don't expect to be here.

```
/lovelace/echo/sequencing/mgx/
# Folders we want
details/
rawfastq/
submissions/
# Folders we don't want
RowlandMetaG/
RowlandRunNS39-remainder/
batch009/
batch010/
```

All these folders seem to have rawfastq files which should already be saved in `mgx/rawfastq/`
but let's double check before we delete important sequencing data.

```sh
$ ls RowlandMetaG/ >> rmg.txt
$ ls RowlandRunNS39-remainder/ >> rmg.txt
$ ls batch009/rawfastq/ >> rmg.txt
$ ls batch010/RowlandRunNS42_part1 >> rmg.txt
$ ls batch010/RowlandRunNS42_part2 >> rmg.txt
$ ls batch010/RowlandRunNS42_part3 >> rmg.txt
$ ls batch010/RowlandRunNS42_part4 >> rmg.txt
$ ls batch010/RowlandRunNS42_part5 >> rmg.txt
$ ls batch010/RowlandRunNS42_part6 >> rmg.txt

$ for f in `cat rmg.txt`; do if [ ! -f rawfastq/$f ]; then echo "$f is missing"; fi done
```
Many files seem to be missing here but - and _ don't match on some file names. Fix and then check again

```sh
$ for f in `cat rmg.txt`; do if [ ! -f rawfastq/$f ]; then echo $f; fi done >> missing.txt; fi done
$ cat missing.txt | perl -pe 's/^([CM]\d+)_(\d+[FE])_(\d+[A-Z]_S)/\1-\2-\3/' > missing_fix.txt
$ for f in `cat missing_fix.txt`; do if [ ! -f rawfastq/$f ]; then echo "$f is missing"; fi done
M0742-2F-1A_S13_L001_R1_001.fastq.gz is missing
M0742-2F-1A_S13_L001_R2_001.fastq.gz is missing
M0742-2F-1A_S13_L002_R1_001.fastq.gz is missing
M0742-2F-1A_S13_L002_R2_001.fastq.gz is missing
M0742-2F-1A_S13_L003_R1_001.fastq.gz is missing
M0742-2F-1A_S13_L003_R2_001.fastq.gz is missing
M0742-2F-1A_S13_L004_R1_001.fastq.gz is missing
M0742-2F-1A_S13_L004_R2_001.fastq.gz is missing
061F-1-8-23-17-O_S24_L001_R1_001.fastq.gz is missing
061F-1-8-23-17-O_S24_L001_R2_001.fastq.gz is missing
061F-1-8-23-17-O_S24_L002_R1_001.fastq.gz is missing
061F-1-8-23-17-O_S24_L002_R2_001.fastq.gz is missing
061F-1-8-23-17-O_S24_L003_R1_001.fastq.gz is missing
061F-1-8-23-17-O_S24_L003_R2_001.fastq.gz is missing
061F-1-8-23-17-O_S24_L004_R1_001.fastq.gz is missing
061F-1-8-23-17-O_S24_L004_R2_001.fastq.gz is missing
061F-2-8-23-17-O_S36_L001_R1_001.fastq.gz is missing
061F-2-8-23-17-O_S36_L001_R2_001.fastq.gz is missing
061F-2-8-23-17-O_S36_L002_R1_001.fastq.gz is missing
061F-2-8-23-17-O_S36_L002_R2_001.fastq.gz is missing
061F-2-8-23-17-O_S36_L003_R1_001.fastq.gz is missing
061F-2-8-23-17-O_S36_L003_R2_001.fastq.gz is missing
061F-2-8-23-17-O_S36_L004_R1_001.fastq.gz is missing
061F-2-8-23-17-O_S36_L004_R2_001.fastq.gz is missing
066F-8-1-17-O_S59_L001_R1_001.fastq.gz is missing
066F-8-1-17-O_S59_L001_R2_001.fastq.gz is missing
066F-8-1-17-O_S59_L002_R1_001.fastq.gz is missing
066F-8-1-17-O_S59_L002_R2_001.fastq.gz is missing
066F-8-1-17-O_S59_L003_R1_001.fastq.gz is missing
066F-8-1-17-O_S59_L003_R2_001.fastq.gz is missing
066F-8-1-17-O_S59_L004_R1_001.fastq.gz is missing
066F-8-1-17-O_S59_L004_R2_001.fastq.gz is missing
117D-1-8-24-17-O_S71_L001_R1_001.fastq.gz is missing
117D-1-8-24-17-O_S71_L001_R2_001.fastq.gz is missing
117D-1-8-24-17-O_S71_L002_R1_001.fastq.gz is missing
117D-1-8-24-17-O_S71_L002_R2_001.fastq.gz is missing
117D-1-8-24-17-O_S71_L003_R1_001.fastq.gz is missing
117D-1-8-24-17-O_S71_L003_R2_001.fastq.gz is missing
117D-1-8-24-17-O_S71_L004_R1_001.fastq.gz is missing
117D-1-8-24-17-O_S71_L004_R2_001.fastq.gz is missing
126H-7-25-17-O_S35_L001_R1_001.fastq.gz is missing
126H-7-25-17-O_S35_L001_R2_001.fastq.gz is missing
126H-7-25-17-O_S35_L002_R1_001.fastq.gz is missing
126H-7-25-17-O_S35_L002_R2_001.fastq.gz is missing
126H-7-25-17-O_S35_L003_R1_001.fastq.gz is missing
126H-7-25-17-O_S35_L003_R2_001.fastq.gz is missing
126H-7-25-17-O_S35_L004_R1_001.fastq.gz is missing
126H-7-25-17-O_S35_L004_R2_001.fastq.gz is missing
127F-7-25-17-O_S47_L001_R1_001.fastq.gz is missing
127F-7-25-17-O_S47_L001_R2_001.fastq.gz is missing
127F-7-25-17-O_S47_L002_R1_001.fastq.gz is missing
127F-7-25-17-O_S47_L002_R2_001.fastq.gz is missing
127F-7-25-17-O_S47_L003_R1_001.fastq.gz is missing
127F-7-25-17-O_S47_L003_R2_001.fastq.gz is missing
127F-7-25-17-O_S47_L004_R1_001.fastq.gz is missing
127F-7-25-17-O_S47_L004_R2_001.fastq.gz is missing
189E-12-2-17-O_S96_L001_R1_001.fastq.gz is missing
189E-12-2-17-O_S96_L001_R2_001.fastq.gz is missing
189E-12-2-17-O_S96_L002_R1_001.fastq.gz is missing
189E-12-2-17-O_S96_L002_R2_001.fastq.gz is missing
189E-12-2-17-O_S96_L003_R1_001.fastq.gz is missing
189E-12-2-17-O_S96_L003_R2_001.fastq.gz is missing
189E-12-2-17-O_S96_L004_R1_001.fastq.gz is missing
189E-12-2-17-O_S96_L004_R2_001.fastq.gz is missing
229D-8-4-17-O_S11_L001_R1_001.fastq.gz is missing
229D-8-4-17-O_S11_L001_R2_001.fastq.gz is missing
229D-8-4-17-O_S11_L002_R1_001.fastq.gz is missing
229D-8-4-17-O_S11_L002_R2_001.fastq.gz is missing
229D-8-4-17-O_S11_L003_R1_001.fastq.gz is missing
229D-8-4-17-O_S11_L003_R2_001.fastq.gz is missing
229D-8-4-17-O_S11_L004_R1_001.fastq.gz is missing
229D-8-4-17-O_S11_L004_R2_001.fastq.gz is missing
240C-1-9-19-17-O_S95_L001_R1_001.fastq.gz is missing
240C-1-9-19-17-O_S95_L001_R2_001.fastq.gz is missing
240C-1-9-19-17-O_S95_L002_R1_001.fastq.gz is missing
240C-1-9-19-17-O_S95_L002_R2_001.fastq.gz is missing
240C-1-9-19-17-O_S95_L003_R1_001.fastq.gz is missing
240C-1-9-19-17-O_S95_L003_R2_001.fastq.gz is missing
240C-1-9-19-17-O_S95_L004_R1_001.fastq.gz is missing
240C-1-9-19-17-O_S95_L004_R2_001.fastq.gz is missing
240C-2-9-19-17-O_S12_L001_R1_001.fastq.gz is missing
240C-2-9-19-17-O_S12_L001_R2_001.fastq.gz is missing
240C-2-9-19-17-O_S12_L002_R1_001.fastq.gz is missing
240C-2-9-19-17-O_S12_L002_R2_001.fastq.gz is missing
240C-2-9-19-17-O_S12_L003_R1_001.fastq.gz is missing
240C-2-9-19-17-O_S12_L003_R2_001.fastq.gz is missing
240C-2-9-19-17-O_S12_L004_R1_001.fastq.gz is missing
240C-2-9-19-17-O_S12_L004_R2_001.fastq.gz is missing
296E-7-26-17-O_S23_L001_R1_001.fastq.gz is missing
296E-7-26-17-O_S23_L001_R2_001.fastq.gz is missing
296E-7-26-17-O_S23_L002_R1_001.fastq.gz is missing
296E-7-26-17-O_S23_L002_R2_001.fastq.gz is missing
296E-7-26-17-O_S23_L003_R1_001.fastq.gz is missing
296E-7-26-17-O_S23_L003_R2_001.fastq.gz is missing
296E-7-26-17-O_S23_L004_R1_001.fastq.gz is missing
296E-7-26-17-O_S23_L004_R2_001.fastq.gz is missing
499A-8-25-17-O_S48_L001_R1_001.fastq.gz is missing
499A-8-25-17-O_S48_L001_R2_001.fastq.gz is missing
499A-8-25-17-O_S48_L002_R1_001.fastq.gz is missing
499A-8-25-17-O_S48_L002_R2_001.fastq.gz is missing
499A-8-25-17-O_S48_L003_R1_001.fastq.gz is missing
499A-8-25-17-O_S48_L003_R2_001.fastq.gz is missing
499A-8-25-17-O_S48_L004_R1_001.fastq.gz is missing
499A-8-25-17-O_S48_L004_R2_001.fastq.gz is missing
520A-1-10-12-17-O_S60_L001_R1_001.fastq.gz is missing
520A-1-10-12-17-O_S60_L001_R2_001.fastq.gz is missing
520A-1-10-12-17-O_S60_L002_R1_001.fastq.gz is missing
520A-1-10-12-17-O_S60_L002_R2_001.fastq.gz is missing
520A-1-10-12-17-O_S60_L003_R1_001.fastq.gz is missing
520A-1-10-12-17-O_S60_L003_R2_001.fastq.gz is missing
520A-1-10-12-17-O_S60_L004_R1_001.fastq.gz is missing
520A-1-10-12-17-O_S60_L004_R2_001.fastq.gz is missing
520A-2-10-12-17-O_S72_L001_R1_001.fastq.gz is missing
520A-2-10-12-17-O_S72_L001_R2_001.fastq.gz is missing
520A-2-10-12-17-O_S72_L002_R1_001.fastq.gz is missing
520A-2-10-12-17-O_S72_L002_R2_001.fastq.gz is missing
520A-2-10-12-17-O_S72_L003_R1_001.fastq.gz is missing
520A-2-10-12-17-O_S72_L003_R2_001.fastq.gz is missing
520A-2-10-12-17-O_S72_L004_R1_001.fastq.gz is missing
520A-2-10-12-17-O_S72_L004_R2_001.fastq.gz is missing
544A-11-2-17-O_S84_L001_R1_001.fastq.gz is missing
544A-11-2-17-O_S84_L001_R2_001.fastq.gz is missing
544A-11-2-17-O_S84_L002_R1_001.fastq.gz is missing
544A-11-2-17-O_S84_L002_R2_001.fastq.gz is missing
544A-11-2-17-O_S84_L003_R1_001.fastq.gz is missing
544A-11-2-17-O_S84_L003_R2_001.fastq.gz is missing
544A-11-2-17-O_S84_L004_R1_001.fastq.gz is missing
544A-11-2-17-O_S84_L004_R2_001.fastq.gz is missing
weak_failed is missing
weak_failed is missing
```
 All these files have either been renamed (ie `M0742_-2F-1A`) or are oral samples from `batch001`.

 ```sh
# remove remaining folders
 $ rm -r batch009/
 $ rm -r batch010/
 $ rm -r RowlandRunNS39-remainder/

# move oral samples to new oral_mgx/ folder
$ cd sequencing/
$ mkdir oral_mgx/
$ mv mgx/RowlandMetaG/ oral_mgx/
$ mv oral_mgx/RowlandMetaG/ oral_mgx/rawfastq/
# some renamed fecal samples remain, remove those
$ ls rawfastq/*fastq.gz | grep -E "^rawfastq\/C" | xargs rm
```

Finally, we want to delete all the temporary files we created in this process as well as the remaining miscellaneous `.txt` files.

```sh
$ cd mgx/
$ rm batch001_missing.txt missing.txt missing_fix.txt rmg.txt
$ rm mothers_fastq.txt nobatch.txt rawfastq_batch005.txt p1.txt
```

We want to move the remaining miscellaneous files to `echo/attic/`.

```sh
$ mkdir /lovelace/echo/attic/sequencing/
$ cd sequencing/mgx/
$ mv IMR-SampleSubmissionSheet_v8\(Rowland_* /lovelace/echo/attic/sequencing/
$ mv *.xlsx /lovelace/echo/attic/sequencing/
$ mv ECHO_May2018_mapping.txt /lovelace/echo/attic/sequencing/
```

## Rsyncing from `lovelace` to `franklin` and `ntm`

Ran into difficulties and eventually manually removed extraneous files from `franklin` and `ntm` and then rsync'd across one final time. (As of Jan 6, 2020)
