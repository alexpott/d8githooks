# Drupal core committer git hooks

## About this project
This project performs automated checks (e.g. file permissions, PHP/CSS/JS coding standards) before/after performing a commit, to ensure regressions are not accidentally committed.

## Project setup

_How do I, as a developer, start working on the project?_

This project depends on:

1. [git] (https://git-scm.com/downloads)
1. [composer](https://getcomposer.org/download/)
1. Drupal 8
1. [Node.js](https://nodejs.org/en/download/)
1. [eslint](http://eslint.org/docs/user-guide/getting-started)
1. Running composer install in the root directory of the repository you have checked out

### Optional dependencies
1. [Homebrew](http://brew.sh/)
1. pbcopy & pbpaste (yep because I use OS X)
1. cowsay (```brew install cowsay```)


## How to install

1. Clone the respository into any directory; for example your home directory: ````cd ~; git clone https://github.com/alexpott/d8githooks.git````
1. Navigate to the ````.git/hooks```` directory inside the Drupal 8 clone from which you commit patches ````cd ~/Sites/8.x/.git/hooks````
1. Create symlinks to pre-commit and post-commit files ````ln -sfn ~/d8githooks/pre-commit pre-commit; ln -sfn ~/d8githooks/post-commit post-commit````
1. Mark the hooks as executable. ````chmod u+x pre-commit; chmod u+x post-commit````

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
git pre-commit check failed: file core/core.services.yml should be 644 not 777`
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

   `composer install`

3. From the root of the repository, run:

   ````vendor/bin/phpcs --standard="core/phpcs.xml.dist" core/path/to/file.php````

   You can confirm that the check is working properly by (e.g.) adding an unused use statement to the file and ensuring it raises an error.
   
Similarly, to use phpcbf to fix a file, run:

````vendor/bin/phpcs --standard="core/phpcs.xml.dist" core/path/to/file.php````

### Troubleshooting eslint

`sudo npm update -g`

## How to use
@todo something about what standard message(s) to copy/paste.
