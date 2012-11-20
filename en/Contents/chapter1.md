# What is atoum ? #

atoum is a unit testing framework, like PHPUnit or SimpleTest.

atoum distinguished itself as :
*    It is modern and based on the last PHP versions
*    It is quite simple and straightforward, with a very limited learning curve
*    It is intuitive : its API wants to be as close as possible as a natural language

## Download & Install ##

For now, atoum is not tagged with a version number. If you want to use atoum, just download the last
stable version. atoum aims to provide backward compatibility anyway.

You can install atoum by 5 ways :
*   [As a PHAR archive](#archive-phar)
*   [Using composer](#composer)
*   [Cloning github repository](#github)
*   [Using symfony 1 plugin](#plugin-symfony1)
*   [Using Symfony 2 bundle](#bundle-symfony2)
*   [Using zend framework 2 component](#component-zend-framework-2).

### PHAR

atoum is distributed as a PHAR archive, an archive format dedicated to PHP, available since PHP 5.3.

You can download the latest stable version of atoum directly from the official website here :
http://downloads.atoum.org/nightly/mageekguy.atoum.phar

### Update

Updating phar archive is quite simple, you have to launch this command:

    [shell]
    php -d phar.readonly=0 mageekguy.atoum.phar --update

**Note**: Updating atoum need to edit PHAR archive, by default php configuration doesn't allow it. That's why we have to use "-d phar.readonly=0".

If a newer version exists, it'll be automatically downloaded and installed on archive.

    [shell]
    php -d phar.readonly=0 mageekguy.atoum.phar --update
    Checking if a new version is available... Done !
    Update to version 'nightly-1568-201210311708'... Done !
    Enable version 'nightly-1568-201210311708'... Done !
    Atoum was updated to version 'nightly-1568-201210311708' successfully !

If there is no newer version, atoum will stop immediately.

    [shell]
    php -d phar.readonly=0 mageekguy.atoum.phar --update
    Checking if a new version is available... Done !
    There is no new version available !

atoum doesn't ask any confirmation to user because it's easy to come back to previous version.

#### List versions contained on archive

To show versions contained on archive, you have to call argument --list-available-versions, or -lav.

    [shell]
    php mageekguy.atoum.phar -lav
    nightly-941-201201011548
    * nightly-1568-201210311708

List of previous versions is shown. Current version is preceded of "*".

#### Edit current version

To activate an other version, simply use argument --enable-version, or -ev, and then the name of version to use.

    [shell]
    php -d phar.readonly=0 mageekguy.atoum.phar -ev DEVELOPMENT

**Note**: Changing current version need to edit PHAR archive, by default php configuration doesn't allow it. That's why we have to use "-d phar.readonly=0".

#### Delete old versions

Over time, archive can contain many versions of atoum which are not used.

To delete them, you have to use argument --delete-version, or -dv, and then the name of version to delete:

    [shell]
    php -d phar.readonly=0 mageekguy.atoum.phar -dv nightly-941-201201011548

**Note**: You cannot delete current version.
**Note**: Changing current version need to edit PHAR archive, by default php configuration doesn't allow it. That's why we have to use "-d phar.readonly=0".

### Composer

[Composer](http://getcomposer.org/) is a tool for dependency management in PHP.

To install atoum through composer, you must install composer

    [shell]
    curl -s https://getcomposer.org/installer | php

Then, create a file named composer.json who contains

    [json]
    {
        "require": {
            "mageekguy/atoum": "dev-master"
        }
    }

And finally execute

    [shell]
    php composer.phar install

### Github

If you want to use atoum directly from it's sources, you can clone or fork its git repository on
github : git://github.com/mageekguy/atoum.git

### symfony 1 plugin

A plugin is available to use Atoum with symfony 1. Documentation and exemples are available
at the following address :
[https://github.com/agallou/sfAtoumPlugin](https://github.com/agallou/sfAtoumPlugin).

### Symfony 2 Bundle

A Bundle is available to use Atoum with Symfony 2. Documentation and exemples are available
at the following address :
[https://github.com/atoum/AtoumBundle](https://github.com/atoum/AtoumBundle).

### Using zend framework 2 component

A library is available to use Atoum with zend framework 2. Documentation and exemples are available
at the following address :
[https://github.com/blanchonvincent/zend-framework-test-atoum](https://github.com/blanchonvincent/zend-framework-test-atoum).

## A quick overview of atoum's philosophy ##

### Very basic example ###

atoum wants you to write a test class for each class you want to test. As an example, if you want to
test the famous HelloTheWorld class, you'll have to write the test\units\HelloTheWorld test class.

NOTE : atoum is, of course, namespace aware. As an example, to test the Hello\The\World class,
you'll write the \Hello\The\tests\units\World class.

Here is the code of your HelloTheWorld class that we'll be using as a first example. This class will
be located in PROJECT_PATH/classes/HelloTheWorld.php

    [php]
    <?php
    /**
     * The class to be tested
     */
    class HelloTheWorld
    {
        public function getHiAtoum ()
        {
            return 'Hi atoum !';
        }

    }

Now, let's write our first test class. This class will be located in
PROJECT_PATH/tests/HelloTheWorld.php

    [php]
    <?php
    //Your test classes are in a dedicated namespace
    namespace tests\units;

    //You have to include your tested class
    require_once __DIR__.'/../classes/HelloTheWorld.php';

    //You now include atoum, using it’s phar archive
    require_once __DIR__.'/atoum/mageekguy.atoum.phar';

    use \mageekguy\atoum;

    /**
     * Test class for \HelloTheWorld
     * Test classes extends from atoum\test
     */
    class HelloTheWorld extends atoum\test
    {
        public function testGetHiAtoum ()
        {
            // create a new instance of class to test
            $helloToTest = new \Vendor\Project\HelloWorld();

            $this
                // we test than getHiAtoum method return a string.
                ->string($helloToTest->getHiAtoum())
                    // ... and string is what we expect !
                    // Hi atoum !
                    ->isEqualTo('Hi atoum !')
            ;

        }
    }

Now, let's launch the tests

    [bash]
    php -f ./test/HelloTheWorld.php

You will see something like this

    [bash]
    > atoum version nightly-941-201201011548 by Frédéric Hardy (phar:///home/documentation/projects/tests/atoum/mageekguy.atoum.phar/1)
    > PHP path: /usr/bin/php5
    > PHP version:
    => PHP 5.3.6-13ubuntu3.3 with Suhosin-Patch (cli) (built: Dec 13 2011 18:37:10)
    => Copyright (c) 1997-2011 The PHP Group
    => Zend Engine v2.3.0, Copyright (c) 1998-2011 Zend Technologies
    =>     with Xdebug v2.1.2, Copyright (c) 2002-2011, by Derick Rethans
    > tests\units\HelloTheWorld...
    [S___________________________________________________________][1/1]
    => Test duration: 0.01 second.
    => Memory usage: 0.00 Mb.
    > Total test duration: 0.01 second.
    > Total test memory usage: 0.00 Mb.
    > Code coverage value: 100.00%
    > Running duration: 0.16 second.
    Success (1 test, 1/1 method, 2 assertions, 0 error, 0 exception) !

In the above example we tested that the method getHiBob
*    did return a string (step 1),
*    and that this string was equal to « Hi Bob ! » (step 2).

Tests are OK, all is green. You're done, your code is rock solid !

### Rule of Thumb ###

The basics when you’re testing things using atoum are the following :
*    Tell atoum what you want to work on (a variable, an object, a string, an integer, …)
*    Tell atoum the state the element is expected to be in (is equal to, is null, exists, …).


## Use atoum on your IDE

## Sublime Text 2

A [SublimeText 2 plugin](https://github.com/toin0u/Sublime-atoum) allows atoum to run unit tests and display results right in editor.

Informations related to installation and configuration are available on
[author blog](http://sbin.dk/2012/05/19/atoum-sublime-text-2-plugin/).

## VIM

atoum has a plugin which integrate him to VIM.

- Tests can be executed easily
- Report showed on VIM split.
- Navigation on errors and jump to assertion line.

### Installation of atoum plugin in VIM

If you don't use atoum as a PHAR archive, the plugin is located in resources/vim, named atoum.vba.

Otherwhise, you have to launch command in *atoum* to extract file:

    [shell]
    php mageekguy.atoum.phar --extractRessourcesTo path/to/a/directory

Once the extraction is complete, atoum.vba file will be located in path/to/a/directory/resources/vim.
Then, edit this file with VIM.

    [shell]
    vim path/to/atoum.vba

And call command:

    [vim]
    :source %

### atoum plugin usage in VIM

To use plugin, atoum has to be installed and you must be currently editing a file which contains a atoum unit tests.
In this case, launch command:

    [vim]
    :Atoum

Tests are launched, once complete, a report based on configuration file of atoum
(located on ftpplugin/php/atoum.vim on your .vim directory) is generated on a new split.

Obviously, you can define a mapping to launch this command using a key combination.
For example, add this line on your .vimrc

    [vim]
    nnoremap *.php :Atoum

Usage of F12 key on your keyboard in normal mode will call command :Atoum.

### Management of atoum configuration files

You can define an other configuration file for atoum by adding this line to your .vimrc file.

    [vim]
    call atoum#defineConfiguration('/path/to/project/directory', '/path/to/atoum/configuration/file', '.php')


It takes 3 arguments:
* A path to the directory which contains unit tests;
* A path to the atoum configuration file ;
* Unit tests file extensions.

For more details on usage of this plugin, use help command in VIM:

    [vim]
    :help atoum
