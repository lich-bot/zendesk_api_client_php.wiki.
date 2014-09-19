# PHPUnit Setup Instructions

A set of simple instructions to get started using PHPUnit for testing. The guide is written assuming the user is working in a *nixy environment
***

## Installing PHPUnit via Composer

PHPUnit can be installed via composer. Since you are likely using composer for the wrapper itself this makes things simple. If you plan to use PHPUnit in the future or on another project I recommend installing it globally. To do this via composer you can enter the below command and options:

```bash
composer global require "phpunit/phpunit=4.2.*"
```
To use PHPUnit from this install, make sure you add `~/composer/vendor/bin` to your PATH environment variable. See the below example. The line can be added to your `.bashrc` file.

```bash
export PATH="$HOME/.composer/vendor/bin:$PATH"
```

Once you do this you can test by restarting your terminal and typing `which phpunit` and should see the path above.

***
## Using PHPUnit with the API Wrapper

Using PHPUnit with our wrapper is simple. Below is an example of how one can run all of the tests. In the example below the current directory is `vendor/zendesk/zendesk_api_client_php`:

```bash
phpunit --bootstrap ./tests/bootstrap.php -c phpunit.xml tests/Zendesk/API/Tests/
```
For testing with your own Zendesk instance or trial it is recommended you rename phpunit.xml and add the new file to your .gitignore. Fill out the appropriate info with settings appropriate to your own instance of Zendesk and reference this file with the `-c` option.  



