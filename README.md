#openFrameworks Sublime#

This is a collection of small tools I use when working with openFrameworks on the commandline and inside Sublime Text (on Debian). Some parts (like paths) might be specific to my setup but should be easy enough to adapt. I'm open to any suggestions that help make them more general. I'm not 100% sure the color highlighting works with every Sublime Text theme, but would like to make sure it does.

My basic flow when working on a project is as follows;

1. Run the openFrameworks `projectGenerator` to create a new project.
2. Copy the `openFrameworks.sublime-project` file into the project folder (either using a file explorer, or by running `ofgetmake` on the commandline, see below).
3. Open the new project in Sublime Text.
4. Hit `ctrl+b` to build the sketch (select `make and run` if asked what profile to use).

Below you'll find a breakdown of the files and tools I use for this.

##openFrameworks.sublime-project##
##### Sublime Text project file#####
This file contains the configuration needed to compile an openFrameworks script from within Sublime and to add error highlighting to the console output (see [openframeworks-console.tmlanguage](#openframeworks-consoletmlanguage) for more info).

The file contains:

1. The command to build and run the project: `make -j 4 && make RunRelease`.
2. The path to the syntax file used for color highlighting in the console.
3. Settings to use tabs that are four spaces wide.
4. Commented lines for hiding certain files/directories from the sidebar for easy reference.

Copy `openFrameworks.sublime-project` to your `/apps/myapps/projectname` folder and open it in Sublime. You can now use `ctrl+b` to build and run your project. The first time Sublime might ask you what build profile to use; select `make and run`.

**Note:** `make` is used with the `-j 4` argument which tells it to do four jobs simultaneously: you might want to change this number to reflect the number of available cores of your system.

##ofmake##
Commandline alias that simply builds your script and runs it in one go. I mostly use this when I'm browsing old or downloaded projects and quickly want to build them from the console. Add this to your `~/.bashrc` or `~/.zshrc` file.

`alias ofmake="make -j 4 && make RunRelease"`

**Note:** `make` is used with the `-j 4` argument which tells it to do four jobs simultaneously: you might want to change this number to reflect the number of available cores of your system.


##ofgetmake##
Commandline script to quickly copy the makefile and config files from the `emptyExample` project into your current working directory. If you copy the `openFrameworks.sublime-project` file into the `emptyExample` folder, this file will also get copied so you can create a complete set of files with just one command.

Especially useful when you've just cloned the repository of an openFrameworks script that doesn't contain these files and you want to quickly build and run the project. If the folder already contains these files (like `addons.make` file) they will not be overwritten, so it is safe to always run this after cloning. You can quickly download and test a repo using only four commands:

	1. git clone <repo url>
	2. cd <repo name>
	3. ofgetmake
	4. ofmake

The script uses absolute paths to find the files, so you do need to change them if you use a new version of openFrameworks, or in a different location. The current script searches for the files at: `~/Documents/of_v0.9.8_linux64_release/apps/myApps/emptyExample/`

Store the file at `/usr/local/bin/ofgetmake` to be able to access it from anywhere on the commandline.

## openFrameworks-console.tmLanguage ##
##### Colored console output #####
While building or running from within Sublime, all output is shown in its output panel. To get some meaningful color in this output you can copy [openFrameworks-console.tmLanguage](https://github.com/javl/openFrameworks_sublime/blob/master/openFrameworks-console.tmLanguage) to your Sublime config folder. My file is located at `~/.config/sublime-text-3/Packages/User/openFrameworks-console.tmLanguage` folder. If you use a different location, make sure to change the path in your `openFrameworks.sublime-project` file.

The colors are very simple; basically when a certain keyword is found, the entire line of output will be colored:
- `warning`: orange,
- `error`, `Segmentation fault`, `failed`: red
- `notice`: yellow
- `succeeded`, `Finished`: green

Most of the time when your script won't compile you can scroll back to the top of the output and scroll down until you find the first red line: this will often be the main error causing the problem.

The exact colors may depend on the theme you are using.
To change the keyword you need to install `PackageDev`. You can then change the [openFrameworks-console.TAML-tmLanguage](https://github.com/javl/openFrameworks_sublime/blob/master/openFrameworks-console.YAML-tmLanguage) file and generate the `openFrameworks-console.tmLanguage` file with the `PackageDev: Convert (YAML, JSON, PList) to...` command.

![screenshot](https://github.com/javl/openFrameworks_sublime/blob/master/img/console.png)
