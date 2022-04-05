# Authenticated GitHub API with Nuxt 3
This project was created using the following process:

## Pre-requisites
- GitHub [Personal Access Token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) created.

## Project Setup
1. Create a new [Nuxt 3 Minimal Starter project](https://v3.nuxtjs.org/getting-started/installation) in your workspace:

    ```js
    $ npx nuxi init nuxt-github-api
    ```

    You can replace `nuxt-github-api` with a directory name of your choice.

2. Install the [Octokit JS Library](https://github.com/octokit/octokit.js) in this new project:

    ```
    $ npm install @octokit/core
    ```

3. Create a `.env` file in the project root containing your GitHub Personal Access Token:

    ```
    TOKEN="personal-access-token123"
    ```

## API Setup
1. Create a `/server/api/github.js` file in the project root.
2. In `github.js` import the Octokit package and create a new `octokit` object using your `TOKEN`:

    ```js
    import { Octokit } from "@octokit/core";

    const octokit = new Octokit({ auth: process.env.TOKEN });
    ```
3. In the same `github.js` file, now [export a default function](https://v3.nuxtjs.org/docs/directory-structure/server) that will handle API requests and return data from the GitHub API:

    ```js
    export default async (req, res) => {

      const response = await octokit.request('GET /gists?per_page=100', {
        org: "octokit",
        type: "private",
      });

      return response.data;
    }
    ```

4. The GitHub API data should be available at `/api/github`!

## Page Setup
1. Create a `/pages/gists.vue` page in the project root.
2. Inside this `gists.vue` page, call the `/api/github` API you just created using the Composition API:

    ```js
    <script setup>

    const { data: gists } = await useAsyncData("gists", () =>
      $fetch("/api/github"),
    );

    </script>
    ```
3. `gists` now contains an array that can be displayed in `gists.vue` using a loop:

    ```html
    <template>
      <div>
        <h2>Gists</h2>
        <ul>
          <li v-for="gist in gists" :key="gist.id">
            {{ gist.description }}
          </li>
        </ul>
      </div>
    </template>
    ```

---
Based on:

## Nuxt 3 Minimal Starter

We recommend to look at the [documentation](https://v3.nuxtjs.org).

### Setup

Make sure to install the dependencies

```bash
yarn install
```

### Development

Start the development server on http://localhost:3000

```bash
yarn dev
```

### Production

Build the application for production:

```bash
yarn build
```

Checkout the [deployment documentation](https://v3.nuxtjs.org/docs/deployment).