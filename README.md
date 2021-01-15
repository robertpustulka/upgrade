# CakePHP Upgrade tool 

[![Build Status](https://img.shields.io/travis/com/cakephp/upgrade?style=flat-square)](https://travis-ci.com/cakephp/upgrade)

Upgrade tools for CakePHP meant to facilitate migrating from CakePHP 2.x to 3.0.0.

**Warning** This tool is still under development and doesn't handle all aspects of migrating.

## Installation

First clone the 3.x branch of this repository:

```bash
git clone https://github.com/cakephp/upgrade.git -b 3.x
```

After downloading/cloning the upgrade tool, you need to install dependencies with `composer`

```bash
php composer.phar install
```

Once dependencies are installed you can start using the `upgrade` shell.

## Usage

The upgrade tool provides a standalone application that can be used to upgrade
other applications or cakephp plugins. Each of the subcommands accepts a path
that points to the application you want to upgrade.

```bash
cd /path/to/upgrade
bin/cake upgrade all /home/mark/Sites/my-app
bin/cake upgrade skeleton /home/mark/Sites/my-app
```
The first command would run all the tasks at once on `/home/mark/Sites/my-app`,
which is probably the way most people will want to use it.
Additionally the second command would run the `skeleton` task on `/home/mark/Sites/my-app`.
This command is not included in `all` as it is only necessary for apps. Plugins don't need it.

It is recommended that you keep your application in version control, and keep
backups of it before using the upgrade tool.

### Order matters

Several of the commands have dependencies on each other and should be run in a specific order. It
is recommended that you run the following commands first before using other commands:

```bash
bin/cake upgrade locations [path]
bin/cake upgrade namespaces [path]
bin/cake upgrade app_uses [path]
```

Once these three commands have been run, you can use the other commands in any order.
The `all` command already used the right order by default.

## Tasks Available

### locations
Move files/directories around. Run this *before* adding namespaces with the namespaces command.

### namespaces
Add namespaces to files based on their file path. Only run this *after* you have moved files.

### app_uses
Replace App::uses() with use statements

### rename_classes
Rename classes that have been moved/renamed. Run after replacing App::uses().

### rename_collections
Rename HelperCollection, ComponentCollection, and TaskCollection. Will also
rename component constructor arguments and \_Collection properties on all
objects.

### method_names
Updates the method names for a number of methods.

### method_signatures
Updates the method signatures for a number of methods.

### fixtures
Update fixtures to use new index/constraint features. This is necessary before running tests.

### tests
Update test cases regarding fixtures.

### i18n
Update translation functions regarding placeholders.

### skeleton
Add basic skeleton files and folders from the "app" repository.

### prefixed_templates
Move view templates for prefixed actions to prefix subfolder. eg. Users/admin_index.ctp becomes Admin/Users/index.ctp.
By default `admin` prefix is handled, you can run this task for other routing prefixes using `--prefix=other` as well.

## More Available Tasks
See [dereuromark/upgrade](https://github.com/dereuromark/upgrade) repo for a few more on-top tasks available, that have not been merged back into this main one yet.
