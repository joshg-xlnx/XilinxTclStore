# XilinxTclStore


## As a Contributor
1. Get git account \<USER\> and download git on your machine
2. Ask for permission to XilinxTclStore repository by sending e-mail to tclstore@xilinx.com
3. Sign on to github.com
4. Switch to Xilinx at upper left side by your account name \<USER\>
5. Switch to GitShell on your machine

### Checkout the Repository

If you have already cloned the repository then skip this step. Otherwise to pull this repository from within a firewall you will need to congifure http.proxy.  Note this setting may be different for your system.  If you have questions contact your IT network administrator:
```bash
git config --global http.proxy http://proxy:80
```

### Setting up a User Name and Email

```bash
#Create and cd to a working directory, for instance ~/github/
mkdir <WORKING_DIR>
cd <WORKING_DIR>
git config --golbal USER.name <USER>  #github USER name
git config --global USER.email your_email@your_company.com
```

6. Clone the repository

We recommend working off of only the master branches for simplicity.  We do support forking of repositories, which require syncing and merging.  If you are familiar with this methodology you can use it, otherwise stick with working off the master branches of each repo.  You need to clone the Xilinx master repo to your local area.  Don't foget to substitute \<USER\> for your real github account name.:

On Windows

```bash
cd <WORKING_DIR>
git clone https://github.com/Xilinx/XilinxTclStore.git
```

On Linux

```bash
cd <WORKING_DIR>
git clone http://<USER>@github.com/Xilinx/XilinxTclStore.git
```

You will need to enter your github password when prompted.

Now you have cloned the repo directories under "\<WORKING_DIR\>/XilinxTclStore"

cd \<WORKING_DIR\>/XilinxTclStore

```bash
git status
```

7. Check out the entire master branch (you can also check out individual files by passing the specific file name).  Below is the command for checking out the entire default branch.
```bash
git checkout 
```

8. Add your application code to the respective directory.  For a new app, create the directories following the taxonomy tclapp/<YOUR_COMPANY>/<YOUR_APP> (for an example see tclapp/mycompany/myapp), including test code<p>
    For more information on creating application, refer to the following section<p>
    ####My First Vivado Tcl App.

9. Mark files for adding
```bash
cd ./XilinxTclStore
git add tclapp/xilinx/<YOUR_APP>
```

10. Commit to local repository
```bash
git commit -m "your description of the changes"
```

11. Push to \<USER\>/XilinxTclStore in Github
```bash
git push origin <USER>
e.g.
git push origin abeuser # abeuser is my USER branch
```

12. Send Pull Request.  Switch back to the github.com in a browser, and navigate to your repo
https://help.github.com/articles/creating-a-pull-request
Press "Pull Request" button right upper-ish <p>
Add any additiona note if you wish<p>

Done!


## As a Gate Keeper
config proxy (refer to As a Contributor)

config \<USER\> (refer to As a Contributor)

config \<USER\> email (refer to As a Contributor)

config merge option
```bash
git config --global merge.defaultToUpstream true
```

1. Create a reposiroty by clone out XilinxTclStore, skip to next step if repository already exists locally
```bash
On Windows
git clone https://github.com/XilinxInc/XilinxTclStore.git
On Linux
git clone http://USER@github.com/XilinxInc/XilinxTclStore.git
```

2. Update local repo with github master
```bash
git fetch
```

3. Merge any changes
```bash
git merge --ff
```

4. Set up remote to point where the pull request is sent usually USER/USER if this has not been done yet

```bash
git remote add remote_name https://github.com/<USER>/XilinxTclStore.git
e.g.
git remote add raj https://github.com/rajklair/XilinxTclStore.git
```

5. Update local repo with \<USER\> branch
```bash
git fetch remote_name
```

6. Merge changes from \<USER\> branch
```bash
git merge remote_name/remote_branch
e.g.
git merge raj/rajklair
```

7. Fix any merge conlicts

8. Add the changes and commit
```bash
git add .
git commit -m "update notes"
```

9. Run tests and checck content

10. Push to github or go to step 11
```bash
git push origin master
```
geto Step 12

11. Go to Github.com
Pull Request from the \<USER\>.<p>
If everything is good, merge, add comments and close the pull request.<p>
If something is not good, add comments so the requester can make changes.<p>
If something is bad, add comments, reject it and close the pull request.<p>
https://help.github.com/articles/merging-a-pull-request

12. Delete the local repository
```bash
git branch -r -d remote_name/remote_branch
e.g.
git branch -r -d raj/rajklair
```
 Or in browser, delete this branch after merging when prompted.

Done

## My First Vivado Tcl App

### Let Vivado know where your cloned Tcl repository is located

Before you start Vivado set XILINX_TCLAPP_REPO to the location of your cloned repository
```bash
setenv XILINX_TCLAPP_REPO <path>/XilinxTclStore
```
Some of the Tcl app related commmands in Vivado require a Tcl repo to be present so it is important the
env variable is set before you start Vivado.

### Fetch and merge updates to your cloned repository

If you have already cloned the repository, then you may want to fetch and merge the latest updates into your clone.<p>
The lastest and greatest official apps are available in the "master" branch so this is where you want to merge into.<p>

```bash
cd XilinxTclStore
git checkout master
git fetch
git merge
```

### Create your own branch

This is necessary only if you want to add or change existing apps.  All changes you make must be made
on a separate branch so that the repository owner (gate keeper) can pull your branch and look at your changes
before deciding if they meet the criteria for accepting into the master branch.

We recommend using a branch name that is your organization-<USERnm>.
```bash
git branch myorg-johnd
git checkout myorg-johnd
```
If the branch alraedy exists in your clone, the make sure you merge the lastest "master" changes to your branch
```bash
git checkout myorg-johnd
git merge master
```

### Create the Directory Structure

The directory structure should follow _repo_/tclapp/_organization_/_appname_/...
```bash
mkdir -p ./tclapp/mycompany/myapp
cd ./tclapp/mycompany/myapp
```

More directory and testing structure can be found in tclapp/README.

### Create the Package Provider

```bash
vi ./myapp.tcl
```

Change the version and namespace to match your app:
```tcl
# tclapp/mycompany/myapp/myapp.tcl
package require Tcl 8.5

namespace eval ::tclapp::mycompany::myapp {

    # Allow Tcl to find tclIndex
    variable home [file join [pwd] [file dirname [info script]]]
    if {[lsearch -exact $::auto_path $home] == -1} {
    lappend ::auto_path $home
    }

}
package provide ::tclapp::mycompany::myapp 1.0
```


### Customize the App

Your app scripts will not be able to have this same name, but could be placed inside of this package provider file.
If you already have the app created, then copy it into _repo_/tclapp/_organization_/_appname_/.
If you are creating the app from scratch, then:
```bash
vi ./myfile.tcl
```

You need to make sure of couple things:

1. You must have all procs in namespaces, e.g.
```tcl
# tclapp/mycompany/myapp/myfile.tcl
proc ::tclapp::mycompany::myapp::myproc1 {arg1 {optional1 ,}} {
    ...
}
```

2. Add the commands that you would like to export to the top of each file.  It is important
that your are explicit about the commands, in other words, do *not* use wildcards.
```tcl
# tclapp/mycompany/myapp/myfile.tcl
namespace eval ::tclapp::mycompany::myapp {
    # Export procs that should be allowed to import into other namespaces
    namespace export myproc1
}
proc ::tclapp::mycompany::myapp::myproc1 {arg1 {optional1 ,}} {
    ...
}
```

3. You **must** have 4 "meta-comments" which describe your procedure interfaces - inside of the procedures,
and each meta-comment **must** be seperated by new lines (without comments)
```tcl
# tclapp/mycompany/myapp/myfile.tcl
namespace eval ::tclapp::mycompany::myapp {
    # Export procs that should be allowed to import into other namespaces
    namespace export myproc1
}
proc ::tclapp::xilinx::test::myproc1 {arg1 {optional1 ,}} {

    # Summary: A one line summary of what this proc does

    # Argument Usage:
    # -arg1 <arg> : A one line summary of this argument which takes a value indicated by <arg>.
    # [-optional1 <arg> = <opt1_default>] : A one line summary of an optional argument that takes a value and which has a default
    # [-optional2] : A one line summary of an optional arg that does not take a value (aka a flag)

    # Return Value:
    # TCL_OK is returned with result set to a string

    # Categories: xilinxtclstore, projutils


    ...
}
```
Each of these meta-comments is interpreted by Vivado at run-time. It is critical that they be present and correct in your app
or it may not function correctly. The following is a description of each meta-comment.

#### Summary

The text following "Summary:" should be a brief, one-line description of your app.

#### Argument Usage

This is the most complex of the meta-comments. As shown in the above example, there should be one line for each mandatory or
optional arg supported by your app. Optional args should be enclosed within []. Args which should be accompanied by a
value **must** be followed by the literal text <, "arg", and >, indicating to the USER where the value should be placed. The
summary should explain what are the valid values that a USER might use.
You can also specify a default value, which is a value which will be assumed if the USER does not specify the given optional
arg. You can also have optional args that do not take any value (these are often referred to as "flags"). For a flag,
it's presence on the command line implies a value of true, indicating that some optional action should be take by
the app. An exception to this rule is if the name of the flag is prefixed with "no_" (for example, -no_cleanup), in which case
a value of false is implied, indicating that the app will not take some action which by default it normally performs.

You can also specify positional args. A positional arg is for which just a value is specified
and that has no corresponding flag (e.g. -arg1).

Here is a more concrete example:

```tcl
        # Argument Usage:
        # file: Name of  file to generate
        # -owner <arg>: USERname of the owner of the file to be generated.
        # [-date <arg> = <todays_date>]: The date to use in yyyy/mm/dd format, will default to today's date if not specified.
```

This example app has one mandatory positional arg, which is the name of a file it will generate. It also has a mandatory
flag -owner, and an optional arg -date, which has a default value. Assuming this app was called "touch", an example usage might be:

touch /home/joe_USER/myfile -owner root

Use of this command would result in the creation of a file named /home/joe_USER/myfile, owned by root, and with
a file creation date of today.


#### Return Value

Use this meta-comment to specify the possible return values for your app.

#### Categories

Use this meta-comment to specify which categories in the Vivado help system your app should be listed. "Categories:"
should be followed by a comma-separated list. By convention, the first category listed should always be "xilinxtclstore".
Any additional categories are up to you as the app developer.


### Create the Package Index and Tcl Index

```bash
cd ~/XilinxTclStore/tclapp/mycompany/myapp
vivado -mode tcl -nolog -nojournal
```

```tcl
pkg_mkIndex .
auto_mkindex .
```


### Running the Linter

If you are not already in Vivado:
```bash
vivado -mode tcl
```

There should be a lint_files command available at this point:
```tcl
Vivado% lint_files [glob XilinxTclStore/tclapp/mycompany/myapp/*.tcl]
```

Correct anything the linter identifies as a problem.


### Check the App before you Deploy

1. Set XILINX_TCLAPP_REPO to point where the local XilinxTclStore is
```bash
setenv XILINX_TCLAPP_REPO <path>/XilinxTclStore
```
or just the path
```bash
setenv XILINX_TCLAPP_REPO <path>
```
Run Vivado
```bash
vivado -mode tcl
```

When the env variable is set, Vivado automatically adds the location of the repository to the
auto_load path in Tcl.

2. Start testing<p>

Require the package that was provided by the app

```tcl
vivado% package require ::tclapp::mycompany::myapp
```

Optionally import an exported proc

```tcl
namespace import ::tclapp::mycompany::myapp::*
myproc1
...
```

### Setting up a per repository User Name and Email

```bash
cd ~/XilinxTclStore
git config USER.name “johnd”
git config USER.email “johnd@mycompany.com”
```


### Commit Changes

Make sure you commit your \<USER\> branch.

```bash
cd ~/XilinxTclStore
git checkout myorg-johnd
git add .
git status
git commit -m "created myapp for mycompany"
git push origin mycompany
```

