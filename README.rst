gtclone
-------
::

  Usage: gtclone PRESET REPONAME [DIRNAME]

  This command clones a git repository.

  'gtclone' performs the following actions:

  1.  DIRNAME is the destination directory. If not specified, then
      ./REPONAME is taken as DIRNAME.

  2.  If DIRNAME exists:

      2.1  If DIRNAME is already under git version control,
           this script will abort.

      2.2  Otherwise, a warning is given that DIRNAME already exists,
           asking for the user's confirmation to continue.

  3.  The full location of the remote repository is determined based
      on PRESET and the repository name.
      For instructions on how to create and use presets, see the
      comments preceding the function called 'loadPreset' in the
      file 'gt-utils'.

  4.  The repository is cloned in DIRNAME.

  5.  The DIRNAME-local properties:
          git config user.name
          git config user.email
      are set to their respective values as (and if) found in the preset
      file.

  See also: the function 'loadPreset' in 'gt-utils'

gtcommit
--------
::

  Usage: gtcommit [-A] [MSG]

  'gtcommit' performs the following actions:

  1   If the CWD is not under version control
      with git, this script will abort.

  2.  Look for a file named 'precommit' in the
      Current Working Directory (CWD) and run ('source') it,
      if found.

  3.  If the option "-A" is passed, all files in the git
      working tree are added to the index before comitting.
      Not recommended!

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

gtsetssh
--------
::

  Usage: gtsetssh HOST USER

  HOST is one of the following values:

      HOST        URL
      github      github.com

  USER is a username at HOST.

  'gtsetssh' performs the following actions:

  1. Find the template file for the given HOST in the 'presets' subdirectory of
     'gt'. The script aborts if this file is not found.

  2. Generate a new pair of ed25519 public/private keys and save them in
     ~/.ssh as HOST_USER.pub and HOST_USER respectively.

  3. Add the alias HOST_USER to ~/.ssh/config.

  4. Create a file named HOST_USER in ~/.gtpresets with the necessary
     environment variables, obtained from the template file and from the
     command-line arguments.

  After running this script, the user needs to perform the following manual steps:

  1. If the USER account at HOST does not yet exist, create it.

  2. Add an SSH key to the account of USER at HOST, and paste the contents of
     ~/.ssh/HOST_USER.pub into it.

  3. Add a token to the account of USER at HOST, copy its value and paste it at
     the end of the last line ('token=') in ~/.gtpresets/HOST_USER.

  4. You can rename the file HOST_USER in ~/.gtpresets to something shorter.

  All files and directories mentioned here that do not yet exist, will be created
  by the script.

  The script will abort if at least one of the following files already exists:
      ~/.gtpresets/HOST_USER
      ~.ssh/HOST_USER.pub
      ~.ssh/HOST_USER

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

