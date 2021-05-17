---
title: Coding standards
weight: 10
aliases:
  - /1.7/development/coding_standards
---

# Coding standards

Consistency is important, even more so when writing open-source code, since the code belongs to millions of eyeballs, and bug-fixing relies on these teeming millions to actually locate bugs and understand how to solve them.

This is why, when writing anything for PrestaShop, be it a theme, a module or a core patch, you should strive to follow the following guidelines. They are the ones that PrestaShop's developers adhere to, and following them is the surest way to have your code be elegantly integrated in PrestaShop.

In short, code consistency helps keeping the code readable and maintainable.

## General conventions

All files containing code MUST:

* Use only UTF-8 without BOM.
* Use the Unix LF (linefeed) line ending.
* End with a single blank line.

## PHP code conventions

PHP files MUST follow the [PSR-2 standard](https://www.php-fig.org/psr/psr-2/) alongside [Symfony standards](https://symfony.com/doc/3.4/contributing/code/standards.html#structure).

{{% notice note %}}
Although [Yoda conditions](https://en.wikipedia.org/wiki/Yoda_conditions) are suggested, they are not enforced.
{{% /notice %}}

### Making your code follow our coding standards

[PHP CS Fixer](https://cs.symfony.com/) has been configured for the PrestaShop project to help developers to comply with these conventions.

You can run it using the following command:
```bash
php ./vendor/bin/php-cs-fixer fix
```

The prestashop specific configuration file [can be found here](https://github.com/PrestaShop/PrestaShop/blob/develop/.php_cs.dist). Also, you can also use the provided [git pre-commit](https://github.com/PrestaShop/PrestaShop/tree/develop/.github/contrib) sample in order to make sure you never forget to make your code compliant!

### Strict typing
{{< minver v="1.7.7" title="true" >}}

Starting on 1.7.7, all new PHP code should be strictly typed.

This means that all new methods **must** specify a type for all parameters as well as the return type. Similarly, all new classes except interfaces must enforce type strictness via `declare`:

```php
<?php
/** 2007-2020 PrestaShop SA and Contributors... */

declare(strict_types=1);

namespace Foo\Bar;

class MyClass
{
    public function doStuff(string $foo, array $bar): void
    {
    }   
}
```

{{% notice note %}}
**It's important to have all classes declare type strictness.** Since PHP 7 is still inherently weakly typed, it's not enough to have your class declare type strictness to make it _enforce_ type strictness. If the class consuming your code does not enforce type strictness as well, methods called by it will have their parameters silently _coerced_ into the specified type instead of being type checked. [Read this article for more details and examples](https://dev.to/robdwaller/how-php-type-declarations-actually-work-1mm5).
{{% /notice %}}

### Deprecations

Following [Symfony conventions](https://symfony.com/doc/4.4/contributing/code/conventions.html#deprecating-code), method and class deprecations in PrestaShop must be noted by adding the appropriate Phpdoc as well as a deprecation error:

```php
<?php

namespace PrestaShop\Awesome\Path;

@trigger_error(
    sprintf(
        '%s is deprecated since version 1.7.8.0 and will be removed in the next major version.',
        MyClass::class
    ),
    E_USER_DEPRECATED
);

/**
 * @deprecated Since 1.7.8.0 and will be removed in the next major.
 */
class MyClass
{
    /**
     * @deprecated Since 1.7.6.0, use AnotherClass::someNewMethod() instead.
     */
    public function someOldMethod()
    {
        @trigger_error(
            sprintf(
                '%s is deprecated since version 1.7.6.0. Use %s instead.',
                __METHOD__,
                AnotherClass::class . '::someNewMethod()'
            ),
            E_USER_DEPRECATED
        );
    }
}
```

## Javascript code conventions

Javascript files MUST follow the [Airbnb Javascript style guide](https://github.com/airbnb/javascript).

## HTML, CSS (Sass), Twig & Smarty code conventions

HTML, CSS (Sass), Twig and Smarty files MUST follow the [Mark Otto's coding standards](https://codeguide.co/).
Mark is the creator of the [Bootstrap framework](https://getbootstrap.com/).

To help developers to comply with these conventions, [Stylelint](https://stylelint.io/), a stylesheet linter, has been configured in the PrestaShop project. You can find the configuration file [on this repository](https://github.com/PrestaShop/stylelint-config).

Same as if you want to [compile assets]({{< ref "/1.7/development/compile-assets" >}}), you need NodeJS and NPM to run Stylelint.

Starting on {{< minver v="1.7.8" >}}, you can run the linter like this:

```bash
npm run scss-lint
```

You can fix auto-fixable errors using this command:

```bash
npm run scss-fix
```

## License information

All PrestaShop files MUST start with the PrestaShop license block:

### Core files

```
/**
 * Copyright since 2007 PrestaShop SA and Contributors
 * PrestaShop is an International Registered Trademark & Property of PrestaShop SA
 *
 * NOTICE OF LICENSE
 *
 * This source file is subject to the Open Software License (OSL 3.0)
 * that is bundled with this package in the file LICENSE.md.
 * It is also available through the world-wide-web at this URL:
 * https://opensource.org/licenses/OSL-3.0
 * If you did not receive a copy of the license and are unable to
 * obtain it through the world-wide-web, please send an email
 * to license@prestashop.com so we can send you a copy immediately.
 *
 * DISCLAIMER
 *
 * Do not edit or add to this file if you wish to upgrade PrestaShop to newer
 * versions in the future. If you wish to customize PrestaShop for your
 * needs please refer to https://devdocs.prestashop.com/ for more information.
 *
 * @author    PrestaShop SA and Contributors <contact@prestashop.com>
 * @copyright Since 2007 PrestaShop SA and Contributors
 * @license   https://opensource.org/licenses/OSL-3.0 Open Software License (OSL 3.0)
 */
```

### Module files

```
/**
 * Copyright since 2007 PrestaShop SA and Contributors
 * PrestaShop is an International Registered Trademark & Property of PrestaShop SA
 *
 * NOTICE OF LICENSE
 *
 * This source file is subject to the Academic Free License version 3.0
 * that is bundled with this package in the file LICENSE.md.
 * It is also available through the world-wide-web at this URL:
 * https://opensource.org/licenses/AFL-3.0
 * If you did not receive a copy of the license and are unable to
 * obtain it through the world-wide-web, please send an email
 * to license@prestashop.com so we can send you a copy immediately.
 *
 * @author    PrestaShop SA and Contributors <contact@prestashop.com>
 * @copyright Since 2007 PrestaShop SA and Contributors
 * @license   https://opensource.org/licenses/AFL-3.0 Academic Free License version 3.0
 */
```
