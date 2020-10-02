# Fallback for `redis` using `predis`

Provides a metapackage for library needing `redis` 
that fallback to a default implementation when user does not meet
the initial requirements.

## Usage

```shell
composer require "composer-fallback/redis.predis:*"
```

Composer will, by preference:
- check if user has `ext-redis`
- otherwise, fallbacks to `predis/predis`

## How does it works

This package contains 2 versions:

1. The highest [1.1](https://github.com/composer-fallback/redis.predis/blob/1.1/composer.json) needs `ext-redis`.

1. The lowest [1.0](https://github.com/composer-fallback/redis.predis/blob/1.0/composer.json) triggers install of `predis/predis`.

Composer will choose this highest version when user has the extension ext-redis.
Otherwise, composer will choose the lowest version and in that case will 
download the following packages: `predis/predis`.

## What problem does it solve?

You are maintaining a library that need an implementation of `redis`,
but you don't want requiring a specific implementation. 

ie. you need function defined in `redis`, but polyfill exists and would be enough.

When end users requires you library with the following code 
```json
{
    "name": "end-user/app",
    "require": {
      "acme/lib": "^1.0"
    }
}
```

They probably ends with such error

```shell
composer up

Your requirements could not be resolved to an installable set of packages.

  Problem 1
    - Installation request for acme/lib ^1.0 -> satisfiable by acme/lib[1.0].
    - acme/lib 1.0 requires ext-redis * -> no matching package found.
```

You can ask user to install a random package, it works, but the DX is very bad,
and confusing at first.

By using `composer-fallback/redis.predis`, 
users will provide the native implementation 
or fallback to your default choice

### Example of user that meet the preferred requirements

```json
{
    "name": "end-user/app",
    "require": {
        "acme/lib": "^1.0",
        "third-party/provide-implementation": "^1.0"
    }
}
```
```shell
composer up
...
Package operations: 2 installs, 0 updates, 0 removals
  - Installing acme/lib (1.0)
  - Installing composer-fallback/redis.predis (1.1)
```

### Example of user that fallback to your recommendations

```json
{
    "name": "end-user/app",
    "require": {
        "acme/lib": "^1.0"
    }
}
```
```shell
composer up
...
Package operations: 3 installs, 0 updates, 0 removals
  - Installing acme/lib (1.0)
  - Installing composer-fallback/redis.predis (1.0)
  - Installing predis/predis (1.0)
```

## Contributing

This repository is automatically generated. If you want contributing and 
submitting an issue or a Pull Request, please use 
[composer-fallback/generator](https://github.com/composer-fallback/generator).
