# Laravel-Vuetify SPA

> A Laravel-Vuetify SPA starter project template.

*Forked from: https://github.com/cretueusebiu/laravel-vue-spa*

## Features

- Laravel 5.5
- Vue + Vuetify + VueRouter + Vuex + VueI18n + ESlint
- Pages with dynamic import and custom layouts
- Login, register and password reset
- Authentication with JWT
- Socialite integration

## Installation

- `composer install`
- `cp .env.example .env`
- Edit `.env` and set your database connection details
- Run `php artisan key:generate` and `php artisan jwt:secret`
- `php artisan migrate`
- `yarn` / `npm install`

## Usage

### Development

```bash
# build and watch
npm run watch

# serve with hot reloading
npm run hot
```

### Production

```bash
npm run production
```
