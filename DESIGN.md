pyNotmuchFS design document
===========================

pyNotmuchFS is a virtual Maildir++ file system for Notmuch queries written in Python. It intends to bridge the gap between Notmuch and common (remote) mail access methods such as IMAP.

pyNotmuchFS is heavily inspired by [notmuchfs](https://github.com/tsto/notmuchfs) created by Tim Stoakes. It is thought that a Python version of notmuchfs enables more people to experiment with their ideal filesystem API to Notmuch.

Components
----------

A pyNotmuchFS is mounted on a directory (hereafter: `pyNotmuchFS directory`), with a `backing directory` and `mail directory` as arguments. Each of these directories is explained in turn.

### Backing directory

The backing directory contains files which define the directories (file name) and their contents (file content, as notmuch query) found in a mounted pyNotmuchFS directory.

### Mail directory

A Notmuch-compatible mail directory, containing your actual e-mail and the `.notmuch` folder.

### pyNotmuchFS directory

The top level of a (mounted) pyNotmuchFS consists of directories. Two types of directories exist: search directories and tag directories. pyNotmuchFS uses the Maildir++ directory layout, with subdirectories supported by use of a `.` in the name.

Each search directory corresponds to a single file in the backing directory. The contents of each search directory are determined by its associated query, defined by the content of the file in the backing directory.

There are two top-level tagging directories: tag and untag. Each of these directories contains directories corresponding to the full list of tags used in the Notmuch database.

Solutions to common operations
------------------------------

### Searching

  - Searching is possible by creating a file in the backing directory containing a valid Notmuch query as its content. Listing the contents of the folder corresponding to this file in a mounted pyNotmuchFS directory returns the query results as a Maildir.

### Tagging

  - Adding a tag to a message is achieved by moving it to a subdirectory of the top level tag folder.
  - Removing a tag from a messages is achieved by moving it to a subdirectory of the top level untag folder.

List of deferred design decisions
---------------------------------

  - do we support a query corresponding to the top level pyNotmuchFS directory (e.g. by using special name INBOX)?
  - do we support directory creation under tag to support the creation of new tags?
  - which (subset of?) files do we allow in the backing directory for passthrough through pyNotmuchFS? This mostly involves auxiliary files 
  - do we support creation or alteration of queries (and their respective files) over IMAP (and if so, how?)
  - (your idea here?)
