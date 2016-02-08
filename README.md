# Drupal core committer git hooks

## About this project
This project performs automated checks (e.g. file permissions, PHP/CSS/JS coding standards) before/after performing a commit, to ensure regressions are not accidentally committed.

## Project setup

_How do I, as a developer, start working on the project?_

This project depends on:

1. Drupal 8
1. pbcopy & pbpaste (yep because I use OS X)
1. moosay
1. [phpcs](http://pear.php.net/package/PHP_CodeSniffer/) - see https://www.drupal.org/node/1419988 for instructions.
1. [eslint](http://eslint.org/)
1. [phpunit](https://phpunit.de/)


## How to install

1. Clone the respository into any directory; for example your home directory: ````cd ~; git clone https://github.com/alexpott/d8githooks.git````
1. Navigate to the ````.git/hooks```` directory inside the Drupal 8 clone from which you commit patches ````cd ~/Sites/8.x/.git/hooks````
1. Create symlinks to pre-commit and post-commit files ````ln -sfn ~/d8githooks/pre-commit pre-commit; ln -sfn ~/d8githooks/post-commit post-commit````
1. Mark the hooks as executable. ````chmod u+x ~/Sites/8.x/.git/hooks/*````

To test that it's working, introduce a file permissions error and then attempt to commit a text change:

````
cd ~/Sites/8.x
chmod 777 core/core.services.yml
echo 'test' >> core/core.services.yml 
git add core/core.services.yml
git commit -m "Test."
````

You should see the error:

````
git pre-commit check failed: file core/core.services.yml should be 644 not 777`
````

## How to use
@todo something about what standard message(s) to copy/paste.
