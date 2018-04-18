+++
draft = false
title = "Migrate your local PHP 7.2 setup to Homebrew v1.5.*"
date = 2018-04-10T17:14:20+02:00
tags = ["php", "homebrew", "macOS", "environment"]
categories = ["PHP", "Open Source"]
featureimage = ""
menu = ""
+++

Last week, [Hans Puac](https://twitter.com/hanspuac), a colleague of mine, wrote a small guide into our internal company chat on how to migrate your local PHP environment on macOS to the new Homebrew version 1.5.*.
The guide helped a lot of other engineers inside [trivago](https://www.trivago.com/).
I thought it might help more people from the internet.
I asked Hans if I am allowed to share it, and he approved.
So kudos belongs to him.
Here we go:

With Homebrew 1.5.0 the tap `homebrew/php` got deprecated.
They migrated it to `homebrew/core`, but this is changing the installation process completely.
If you want to migrate:

## 1. Cleanup

Remove your currently installed php-packages.
This is not 100% necessary, but I wanted to have it clean.

You can check what packages are installed via `brew list | grep php` for example and remove via `brew remove MYPACKAGE`.
Check for leftovers in `/usr/local/etc/php/` and remove if necessary.
Untap the deprecated repo `brew untap homebrew/php`.
You can check your taps with `brew tap`.
I also had some other taps that I directly untapped as well because those were deprecated:

- `brew untap homebrew/science`
- `brew untap homebrew/versions`

## 2. Update

`brew update`

## 3. Install PHP

Install PHP via `brew install php@7.2`

## 4. Install Extensions

The `intl` is now already built in.
Install all other required extensions via [PECL](https://pecl.php.net/) (gets installed together with PHP).

Installation via pecl requires autoconf (`brew install autoconf`):

- `pecl install xdebug`
- `pecl install redis`
- `pecl install apcu`
- `pecl install memcached`
- `pecl install imagick`
...

### Particular version of an extension

If your app depends on a particular version of an extension, e.g. [redis in v3.1.6](https://pecl.php.net/package/redis) specify the version explicitly via `pecl install redis-3.1.6`.

### Extension `snmp`

If you rely on the [SNMP Extension](https://secure.php.net/manual/en/book.snmp.php), I have to disappoint you right now.
This extension is, state of now (2018-04-10), not part of this PHP build:

> SNMP was excluded from the build because it crashes Apache.

Check out the comments of [SMillerDev](https://github.com/SMillerDev) in [php7.1 extension warnings after migration to core and upgrade #4827](https://github.com/Homebrew/homebrew-php/issues/4827) and [php71: migrate to homebrew/core #4798](https://github.com/Homebrew/homebrew-php/pull/4798).

## 5. Cleanup

To make some disk space free, you may remove old versions of packages by executing `brew cleanup -s`.

## Resources

- [Homebrew 1.5.0 release announcement](https://brew.sh/2018/01/19/homebrew-1.5.0/)
- [Homebrew/homebrew-php @ github](https://github.com/Homebrew/homebrew-php)
- [PHP load and tap errors with latest brew @ brew discourse](https://discourse.brew.sh/t/php-load-and-tap-errors-with-latest-brew/1956/2)
