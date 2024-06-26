# Drupal core committer git hooks

## About this project
This project performs automated checks (e.g. file permissions, PHP/CSS/JS coding standards) before/after performing a commit, to ensure regressions are not accidentally committed.

## Project setup

_How do I, as a developer, start working on the project?_

This project depends on:

1. [git] (https://git-scm.com/downloads)
1. [composer](https://getcomposer.org/download/)
1. Drupal 10
1. Running composer install in the root directory of the repository you have checked out
1. Running yarn install in the core directory of the repository you have checked out

### Optional dependencies
1. [Homebrew](http://brew.sh/) (You can use homebrew to install many of the dependencies above.)
1. pbcopy & pbpaste
1. cowsay (```brew install cowsay```)


## How to install

1. Clone the respository into any directory; for example your home directory: 

   ````
   cd ~; git clone https://github.com/alexpott/d8githooks.git
   ````
   
1. Navigate to the `.git/hooks` directory inside the Drupal 8 clone from which you commit patches 

   ````
   cd ~/Sites/8.x/.git/hooks
   ````

1. Create symlinks to pre-commit and post-commit files:
   
  ```
  ln -sfn ~/d8githooks/pre-commit pre-commit;
  ln -sfn ~/d8githooks/post-commit post-commit;
  ````

1. Mark the hooks as executable. 

   ````
   chmod u+x pre-commit; chmod u+x post-commit
   ````

To test that it's working, introduce a file permissions error and then attempt to commit a text change:

````
cd ~/Sites/8.x
composer install
chmod 777 core/core.services.yml
echo 'test' >> core/core.services.yml 
git add core/core.services.yml
git commit -m "Test."
````

You should see the error:

````
git pre-commit check failed: file core/core.services.yml should be 644 not 777
````

## Troubleshooting

### Using core's Coder dev dependency

If you previously installed PHPCS or Coder globally according to instructions on https://www.drupal.org/node/1419988 and your pre-commit hook starts failing silently, you may need to do:

````
composer global remove drupal/coder
composer global remove phpcs
composer install
````

This change is required because a canonical version of Coder is now added as a core dev dependency.

### Manually checking or fixing a PHP standard

To manually test the standard on a particular file prior to commit:

1. Apply any relevant patch (including fixes, newly configured rules, etc.)
2. From within the repository, run:

   ````
   composer install
   ````

3. From the root of the repository, run:

   ````
   composer run phpcs -- core/path/to/file.php
   ````

   You can confirm that the check is working properly by (e.g.) adding an unused use statement to the file and ensuring it raises an error.
   
   Similarly, to use phpcbf to fix a file, run:

   ````
   composer run phpcbf -- core/path/to/file.php
   ````

   Note that in some cases the enabled core rules are not enough to fix errors correctly! You can also check:

   ````
   vendor/bin/phpcs --standard=Drupal core/path/to/file.php
   ````
   
   Be aware, however, that this may introduce out-of-scope changes as it will run rules that core does not yet comply with.

### Troubleshooting eslint

````
sudo npm update -g
````

If you previously installed yarn from Homebrew or another non-npm package manager:
````
brew uninstall yarn
npm install -g corepack
corepack enable
````

## How to use
@todo something about what standard message(s) to copy/paste.
