::page{title="Hands-on Lab: Archiving and Compressing Files"}


<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-LX0117EN-SkillsNetwork/images/IDSN-logo.png" width="300" alt="cognitiveclass.ai logo">


Estimated time needed: **15** minutes

## Learning Objectives

After completing this lab, you will be able to:

- Create archives for sets of files and folders
- Compress and decompress files
- Extract files and folders from an existing archive

::page{title="About Skills Network Cloud IDE"}

Skills Network Cloud IDE (based on Theia and Docker) provides an environment for hands-on labs for course and project related labs. Theia is an open source IDE (Integrated Development Environment), that can be run on the desktop or on the cloud. To complete this lab, you will be using the Cloud IDE based on Theia.

## Important notice about this lab environment

Please be aware that sessions for this lab environment are not persisted. Thus, every time you connect to this lab, a new environment is created for you, and any data or files you may have saved in a previous session will be lost. To avoid losing your data, plan to complete these labs in a single session.

::page{title="Exercise 1 - File and folder archiving and compression"}

### 1.1. Create and manage file archives

**`tar`**

The `tar` command allows you to pack multiple files and directories into a single archive file.

The following command creates an archive of the entire `/bin` directory and writes the archive to a single file named `bin.tar`.<br>

The options used are as follows:

| Option | Description                     |
| ------ | ------------------------------- |
| -c     | Create new archive file         |
| -v     | Verbosely list files processed  |
| -f     | Archive file name               |

```
tar -cvf bin.tar /bin
```

To see the list of files in the archive, use the `-t` option:

```
tar -tvf bin.tar
```

To untar the archive or extract files from the archive, use the `-x` option:

```
tar -xvf bin.tar
```

Use the `ls` command to verify that the folder `bin` is extracted.

```
ls -l
```

### 1.2. Package and compress archive files

**`zip`**

The `zip` command allows you to compress files.

The following command creates a zip file named `config.zip` consisting of all the files with extension `.conf` in the `/etc` directory.

```
zip config.zip /etc/*.conf
```

The **`-r`** option can be used to zip an entire directory.
The **`-y`** flag to prevent symbolic links from being followed recursively:

The following command creates an archive of the `/bin` directory.

```
zip -ry bin.zip /bin
```

### 1.3. Extract, list, or test compressed files in a ZIP archive

**`unzip`**

The `unzip` command allows you to extract files.

To list the files of the archive `config.zip`, enter the following:

```
unzip -l config.zip
```

The following command extracts all the files in the archive `bin.zip`.

```
unzip -o bin.zip
```

We added the `-o` option to force overwrite in case you run the command more than once.

You should see a folder named `bin` created in your directory.

::page{title="Summary"}

In this lab, you learned that:
- `tar` allows you to pack multiple files and directories into a single archive file
- `zip` allows you to compress files
- `unzip` allows you to extract files

## Authors

Ramesh Sannareddy  
Sam Prokopchuk  

### Other contributors

Rav Ahuja  
Jeff Grossman
<!--
## Change Log

| Date (YYYY-MM-DD) | Version | Changed By        | Change Description                 |
| ----------------- | ------- | ----------------- |----------------------------------- |
| 2023-05-04        | 3.3    | Benny Li      | QA pass            |
| 2023-04-26        | 3.2     | Steve Hord        | QA pass with edits             |
| 2023-04-13        | 3.1     | Nick Yi           | ID Review                          | 
| 2023-01-10        | 3.0     | Jeff Grossman     | Split lab and add new content      |  
| 2021-12-02        | 2.1     | Jeff Grossman     | Review and Update lab              |  
| 2021-11-29        | 2.0     | Sam Prokopchuk    | Update lab contents and split      |  
| 2021-05-30        | 1.0     | Ramesh Sannareddy | Created initial version of the lab |  

Copyright (c) 2021-23 IBM Corporation. All rights reserved.
-->
<h3 align="center"> &#169; IBM Corporation. All rights reserved. <h3/>


