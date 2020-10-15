# Laravel Jetstream Inertia Vue 3

Simple example of a Laravel 8 app using Jetstream and Inertia with Vue 3.

## Steps

1.  Create a new Laravel Jetstream application

    ```bash
    $ laravel new laravel-jetstream-inertia-vue3
    ```

1.  Install Laravel Mix 6 (beta)

    ```bash
    $ npm install laravel-mix@next
    ```

    See https://github.com/JeffreyWay/laravel-mix/blob/master/UPGRADE.md

1.  Update scripts and devDependencies objects in `package.json` file:

    ```json
    {
        "private": true,
        "scripts": {
            "dev": "npx mix",
            "development": "npx mix",
            "watch": "npx mix watch",
            "hot": "npx mix hot",
            "prod": "npx mix --production",
            "production": "npx mix --production"
        },
        "devDependencies": {
            "@inertiajs/inertia": "^0.4.0",
            "@inertiajs/inertia-vue3": "^0.1.0",
            "@vue/compiler-sfc": "^3.0.0",
            "axios": "^0.20.0",
            "laravel-mix": "^6.0.0-beta.5",
            "vue": "^3.0.0",
            "vue-loader": "^16.0.0-beta.8",
            "@tailwindcss/ui": "^0.6.0",
            "cross-env": "^7.0",
            "laravel-jetstream": "^0.0.3",
            "lodash": "^4.17.19",
            "portal-vue": "^2.1.7",
            "postcss-import": "^12.0.1",
            "resolve-url-loader": "^3.1.0",
            "tailwindcss": "^1.8.0"
        }
    }
    ```

1.  Install the new dependencies:

    ```bash
    $ npm i
    ```

1.  Update `webpack.mix.json` file:

    ```js
    const mix = require("laravel-mix");
    const path = require("path");

    mix.postCss("resources/css/app.css", "public/css", [
        require("postcss-import"),
        require("tailwindcss")
    ])
        .js("resources/js/app.js", "public/js")
        .vue({ version: 3 });

    mix.alias({
        "@": path.resolve("resources/js")
    });
    ```

1.  Update `/resources/js/app.js` file to use inertia and vue 3:

    ```js
    import { createApp, h } from "vue";

    import { app, plugin } from "@inertiajs/inertia-vue3";

    const el = document.getElementById("app");

    createApp({
        render: () =>
            h(app, {
                initialPage: JSON.parse(el.dataset.page),
                resolveComponent: name =>
                    import(`@/Pages/${name}`).then(module => module.default)
            })
    })
        .use(plugin)
        .mount(el);
    ```

    See https://github.com/inertiajs/inertia/releases/tag/inertia-vue3%40v0.1.0

1.  Launch npm mix to let it installs additional dependencies:
    ```bash
    $ npm run dev
    ```
1.  Re-launch npm mix:
    ```bash
    $ npm run dev
    ```
    You should obtain an error:
    ```bash
    VueCompilerError: <template v-for> key should be placed on the <template> tag.
    ```
1.  Update `/resources/js/Layouts/AppLayout.vue` file on line 179 to move `:key="team.id"` from `<form>` element to its parent `<template>` element:

    ```js
    <template v-for="team in $page.user.all_teams" :key="team.id">
    <form @submit.prevent="switchToTeam(team)">
    ```

1.  Done 🥳

    ```bash
    DONE  Compiled successfully in 8719ms                                                      4:48:19 PM

    99% done plugins BuildOutputPlugin

    Laravel Mix v6.0.0-beta.10

    ✔ Compiled Successfully in 8719ms
    ┌───────────────────────────────────┬──────────┐
    │ File                              │ Size     │
    ├───────────────────────────────────┼──────────┤
    │ css/app.css                       │ 4.38 MiB │
    ├───────────────────────────────────┼──────────┤
    │ /js/app.js                        │ 1.14 MiB │
    ├───────────────────────────────────┼──────────┤
    │ js/resources_js_Pages_Profile_Sh… │ 367 KiB  │
    ├───────────────────────────────────┼──────────┤
    │ js/resources_js_Pages_API_Index_… │ 289 KiB  │
    ├───────────────────────────────────┼──────────┤
    │ js/resources_js_Pages_API_ApiTok… │ 178 KiB  │
    ├───────────────────────────────────┼──────────┤
    │ js/resources_js_Pages_Profile_Tw… │ 135 KiB  │
    ├───────────────────────────────────┼──────────┤
    │ js/resources_js_Pages_Dashboard_… │ 128 KiB  │
    ├───────────────────────────────────┼──────────┤
    │ js/resources_js_Pages_Profile_Lo… │ 115 KiB  │
    ├───────────────────────────────────┼──────────┤
    │ js/resources_js_Pages_Profile_De… │ 109 KiB  │
    ├───────────────────────────────────┼──────────┤
    │ js/resources_js_Pages_Profile_Up… │ 99 KiB   │
    ├───────────────────────────────────┼──────────┤
    │ js/resources_js_Pages_Profile_Up… │ 82.5 KiB │
    └───────────────────────────────────┴──────────┘
    ```
