# Bootplate

The **Bootplate** template provides a complete starter project that supports source control
and cross-platform deployment out of the box.  It can be used to facilitate both the
creation of a new project and the preparation for its eventual deployment.

## Quick Start with Bootplate

There are two ways to use the Bootplate template to start a new project.  Whether or not
you want your project to live on GitHub will determine which one is right for you.

### The GitHub Way

1. If you want your new project to be created as its own GitHub repository, the first
    step is to fork, or duplicate, the [bootplate template project](https://github.com/enyojs/bootplate).

    Refer to the document on [Dupliforking](Dupliforking) for instructions on how
    to spawn a new repository from the bootplate project.

2. Load `debug.html` in a browser and see "Hello World".

**Note:** Even if you don't plan to "Duplifork" bootplate so you can push back to GitHub, if you pull the bootplate repo from GitHub, you will still need to initialize the `enyo/libs` submodules by running this command from within the bootplate app's directory:

        git submodule update --init

### The Non-GitHub Way

1. If you have downloaded the Enyo source from [enyojs.com](http://enyojs.com), the zip archive should
    contain a copy of the Bootplate template (without the source control hooks) in a top-level directory called `bootplate`.  Locate this folder and open it.

2. Load `debug.html` in a browser and see "Hello World".

## Development

At this point, you would refine your project through the normal cycle of development and testing, starting with the `app.js`/`app.css` files provided in the template.  As you factor your app into more and more JavaScript and CSS files and use libraries with their own `package.js` files, just make sure to include them in your top-level `source/package.js`, and the minify/deploy scripts described below will combine them all into a single js/css file for deployment.

For the purposes of this article, let's assume that you've completed all
of your work on the HelloWorld app, and turn our attention to the deployment process.

## Preparing for Deployment

By following the structure established by the Bootplate template, you set yourself up
for a relatively pain-free experience when it comes time to prepare your finished app for
deployment:

1. Duplicate the project folder and give it a name, e.g., `"myapp-deploy"`.  All the steps
    below will be done using this duplicate folder.  (Keep your development version safe!)

2. Make a deployment build by doing the following:

        * Open a command prompt (Windows) or terminal window (Mac/Linux).

        * On the command line, navigate to the `tools` folder.

        * Run the `deploy.bat` script on Windows, or `deploy.sh` on Mac or Linux.

    Note that the `deploy` script invokes the `minify` script, so it is typically not necessary to call `minify` directly.

    `minify` creates one compressed JavaScript file and one compressed CSS file for your app, which it then writes to a folder called `build`.

    After `minify` completes, `deploy` copies a subset of the project files (including the `build` folder) into a date-stamped `deploy` folder.

3. Open the deployment folder, load `index.html` in a browser, and see "Hello World" (but faster!).

Now your project is ready to deploy to various HTML5-ready targets.  For details about
deploying an app to specific platforms, see [Platform-Specific Deployment](Platform-Specific-Deployment).

## Additional Notes on Bootplate Projects

### Embedded Enyo

Bootplate projects are set up to use _embedded_ enyo. In other words, the Enyo library and other dependencies are stored completely inside the project folder. These resources are relatively small, and keeping all the dependencies together allows the developer to control versions and to easily deploy sealed packages (e.g., PhoneGap builds).

### Submodules for Versioning

Resources from other repositories are included as git submodules. This way, you can control the versions of those resources in your project directly from git (you can lock to a version, update, or revert at will).

In particular, the `enyo` folder and package libraries in the `lib` folder are actually submodules.

### Development vs. Deployment

When developing and debugging your project, it's common to need various source files and helper tools that are not needed in final packages.  For this reason, we have the idea of making an application _deployment_.  A deployment refers to a final production package, ready for inclusion in an app store or other method of distribution.

An important feature of bootplate projects is the relative ease of generating deployments.

### Folder Structure

A bootplate project has the following structure:

	assets/
	build/
	enyo/
	lib/
	source/
	tools/
	debug.html
	index.html

* `assets` contains images or other data files for you projects. Files in this folder are
    intended to be necessary for deployment.

* `build` contains JavaScript and CSS source files that have passed through the Enyo minifier script.  If the `build` folder does not exist, it will be created when the minifier is run.

* `enyo` contains the Enyo framework source files.  This folder is only necessary for debugging, and
    can be deleted for deployment.

* `lib` contains various plugin files.  Individual folders in `lib` can come from various
    sources and may have custom rules for deployment.  In general, `assets` or `images`
    folders are required, and other files are only for debugging.

* `source` contains the code source files or other debug-only materials.

* `tools` contains the `deploy` and `minify` shell scripts.

* `debug.html` loads the application without using any built files; this is generally
    the easiest way to debug.

* `index.html` loads the application using built files only.  If built files are not
    available, it will redirect to `debug.html`.

### Updating the Submodules Manually

If you want to use top-of-tree versions of enyo, layout, and onyx, you can do this with a couple of
git commands:

    git submodule foreach 'git checkout master'
    git submodule foreach 'git pull'

The first command switches each submodule from being pinned to a specific commit to being on the master 
branch, while the second pulls any new source changes from GitHub.  You can also manually check out specific
tags or branches if you want.

If you want to use stable code, the Enyo team manually updates the submodules links from time to time as we
make updates to Bootplate, so you can just pull the bootplate repo and then use `git submodule update` to refresh
your local tree.