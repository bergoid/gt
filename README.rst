gtclone
-------
::

  Usage: gtclone PRESET [REPONAME]

  This command clones a git repository in the Current Working Directory (CWD).

  'gtclone' performs the following actions:

  1.  If the CWD is already under git version control, this script
      will abort.

  2.  If the CWD is not empty, this script will abort.

  3.  If the optional argument REPONAME is specified,
      this is used as the repository name. Otherwise
      the name of the CWD is used.

  4.  The full location of the repository is determined based
      on PRESET and the repository name.
      For instructions on how to create and use presets, see the
      comments preceding the function called 'loadPreset' in the
      file 'gt-utils'.

  5.  The repository is cloned in the CWD.

  See also: the function 'loadPreset' in 'gt-utils'

gtcommit
--------
::

  Usage: gtcommit [-A] [MSG]

  'gtcommit' performs the following actions:

  1   If the CWD is not under version control
      with git, this script will abort.

  2.  Look for an executable file named 'precommit' in the
      Current Working Directory (CWD) and run it, if found.

  3.  If the option "-A" is passed, all files in the git working tree
      are updated to the index. Not recommended!

  4.  The files are committed with commit message "MSG". If no
      argument is given, the commit message must be typed into
      a text editor that will open automatically.

  See also: 'gtnew', 'gtclone'

gtnew
-----
::

  Usage: gtnew PRESET [REMOTEREPONAME]

  This command initializes the Current Working Directory (CWD)
  for version control with git and creates a remote repository.

  'gtnew' performs the following actions:

  1   If the CWD is already under version control with git,
      this script will abort.

  2.  If the CWD is empty, an empty file called 'placeholder' is
      created.

  3.  If the optional argument REMOTEREPONAME is specified, this is
      used as the remote repository name. Otherwise the name of the
      CWD is used.

  4.  'gtnew' creates a repository on the remote
      location. If this action fails for some reason, the
      script aborts.
      The full location of the repository is determined based
      on PRESET and REMOTEREPONAME.
      For instructions on how to create and use presets, see the
      comments preceding the function called 'loadPreset' in the
      file 'gt-utils'.

  5.  A .git directory is initialized in your CWD, and the 'origin'
      property in .git/config is set to the repo created on the
      remote location.

  6.  The script 'gtcommit' is called to add the entire git working
      tree to the staging area (option '-A') and commit it with the
      message 'First commit'.

  7.  The first commit is pushed to the origin repository.

  See also: 'gtclone', 'gtcommit'

gttag
-----
::

  Usage: gttag TAGNAME

  This command tags the commit referenced by HEAD and pushes the tag to the remote origin.

  'gttag' performs the following actions:

  1.  The script aborts if:
          1.1. the Current Working Directory (CWD) is not under version control with git
          1.2. the git working tree has no remote upstream branch
          1.3. there are uncommited changes in the git working tree
          1.4. some commits are not yet pushed to the remote origin

  2.  An annotated tag is created with the name 'TAGNAME'. The tag message is also set to TAGNAME.

  3.  All tags that are not yet on the remote origin are pushed there.

