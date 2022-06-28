---
title: Contribute
---

<div class="page-header">
    <h1>How to contribute ?</h1>
</div>

Thelia project is hosted on [GitHub](https://github.com/thelia/thelia). For contributing you just have to fork the project
and submit [Pull Request](https://help.github.com/articles/using-pull-requests) or [Issues](https://github.com/thelia/thelia)

## Coding Standard

Thelia follow [PSR-I](http://www.php-fig.org/psr/psr-1/) and [PSR-2](http://www.php-fig.org/psr/psr-2/) therefore you
must follow this rules. Don't worry, you can use some tools for doing this like the
[PHP Coding Standards Fixer](http://cs.sensiolabs.org/)

## Pull Request

[Creating a Pull request](https://help.github.com/articles/creating-a-pull-request) is the better way for submitting a
patch but there are some rules to follow. First of all, fork [Thelia](https://github.com/thelia/thelia) repo and create
a new branch, never work on the master branch, use it only for syncing with [Thelia](https://github.com/thelia/thelia) repo.

```
$ git checkout -b new-branch master
```

After finishing your modification you have to rebase your branch and push it to your repo

```
$ git remote add upstream https://github.com/thelia/thelia.git
$ git checkout master
$ git pull --ff-only upstream master
$ git checkout new-branch
$ git rebase master
$ git push origin new-branch
```

Next and last step, submit a Pull Request as indicated in the [GitHub documentation](https://help.github.com/articles/creating-a-pull-request).

If you want to do more, read this usefull blog post : [http://williamdurand.fr/2013/11/20/on-creating-pull-requests/](http://williamdurand.fr/2013/11/20/on-creating-pull-requests/)


## SQL scripts modification

If you submit modifications that adds new data or change the structure of the database, please read the following documentation.

### Changes in Thelia model

If you have changed the `schema.xml` file which is the Propel schema of Thelia models, you should generate the new `thelia.sql` file and the base classes of Propel.

You should execute this command at the root directory of your Thelia website :

```sh
# generates classes
bin/propel build -v --input-dir=local/config/ --output-dir=core/lib/ --enable-identifier-quoting
# generates thelia.sql
bin/propel sql:build -v --input-dir=local/config/ --output-dir=setup/
rm setup/sqldb.map
```

### Generate SQL for default data and update SQL scripts

These SQL files should not be modified directly as they are generated by a Thelia command. Instead, you should edit the smarty templates. The first one is the file `setup/insert.sql.tpl` that is used to generate the `insert.sql` file. Others are located in `setup/update/tpl` and are used to generate all SQL update files.

This templates only differ from sql for **i18n** tables. *But we could imagine other uses with Smarty*.  
A typical application :

```smarty
...

INSERT INTO  `module_i18n` (`id`, `locale`, `title`, `description`, `chapo`, `postscriptum`) VALUES
{foreach $locales as $locale}
  (@max_id+1, '{$locale}', {intl l='Navigation block' locale=$locale}, NULL,  NULL,  NULL),
  (@max_id+2, '{$locale}', {intl l='Currency block' locale=$locale}, NULL,  NULL,  NULL),
  ...
  (@max_id+12, '{$locale}', {intl l='New Products block' locale=$locale}, NULL,  NULL,  NULL),
  (@max_id+13, '{$locale}', {intl l='Products offer block' locale=$locale}, NULL,  NULL,  NULL){if ! $locale@last},{/if}

{/foreach}
;

...
```

- `{foreach $locales as $locale}` is used to iterate on the list of locales : en\_US, fr\_FR, ...
- `{intl l='Navigation block' locale=$locale}` is used to display the translation corresponding to the `l` attribute. This `intl` function
differs from the classic one used in Thelia. If the translation does not exist, no fallbacks will be used by default.
The text will be escaped for SQL and quotes will be placed around the input string. If the string is empty then it will be replaced by a `NULL` value.
The attribute `in_string="1"` is used to disable the placement of quotes around the string. The attribute `use_default="1"` allow you to use the `l`
attribute as a fallback if the translation does not exist.
- don't forget to use the `{if ! $locale@last},{/if}` before the `{/foreach}` otherwise your SQL will not be valid.

Keep attention on brackets `{` or `}` that is used by smarty. You can use `{ldelim}`, `{rdelim}` or `{literal}...{/literal}` to escape non smarty code.

To translate the new string, you can use the translation page in the back office.

I you modify templates you have to regenerate all sql files, you can use this Thelia command : `php Thelia generate:sql`

You can also limit to a specific list of locales if you use `locales` parameter.

Actually, as all languages are not fully translated we use this command line to generate SQL files :

```sh
php Thelia generate:sql --locales='de_DE,en_US,es_ES,fr_FR'
```


## How to contribute new or update Thelia translations

Translations are contributed by Thelia users worldwide. The translation work is coordinated at [Crowdin](http://crowdin.com).  
The Thelia project is located at <http://translate.thelia.net/>.

Translations for **non english** languages should only be done on <http://translate.thelia.net/>, not in a Thelia development website and submitted to us with a pull request on GitHub.  
During the development stage, only english strings should be used inside Thelia and submitted with a pull request.  
Prior to any release, Thelia maintainers will make an announcement and we'll have a couple of weeks
of string freeze in order to give people time to complete the translations.
Once translations are done, Thelia maintainers will integrate all translations in Thelia.

If you want to contribute to translation or want to discuss specific translations, go to the [Thelia project page](http://translate.thelia.net/).

If you would like to help out with translating or adding a language that isn’t yet translated, here’s what to do:

- Visit the [Thelia project page](http://translate.thelia.net/).
- Sign up at  [Crowdin](http://crowdin.com) or log in if you already have an account.
- On the Thelia project page, click the **Join Translation Project** button.
- Choose the language you want to work on, or – in case the language doesn’t exist yet – request a new language by clicking on the **Contact** link of one of the managers of the project.
- Then Select a file in the list and start translating. if you encounter any problems, please consult [Crowdin Knowledge Base](https://support.crowdin.com/) or open a [new discussion on Thelia project page](http://translate.thelia.net/project/thelia/discussions).