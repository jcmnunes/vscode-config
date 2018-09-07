# Gatsby setup (with VS Code)

In this guide, we will make Gatsby development setup in VS Code. Some things we want to achieve are:

- SASS with autoprefixer support
- CSS modules
- Site metadata
- ESLint and Prettier

## Create a new site

We will start with the bare essentials needed for a Gatsby site by using the [hello world starter](https://github.com/gatsbyjs/gatsby-starter-hello-world):

```
gatsby new sitename https://github.com/gatsbyjs/gatsby-starter-hello-world
```

## SASS with autoprefixer support

We need to first install the sass plugin. We will be using [gatsby-plugin-postcss-sass](https://www.npmjs.com/package/gatsby-plugin-postcss-sass):

```terminal
npm install --save-dev gatsby-plugin-postcss-sass
```

Then, we need to create a `gatsby-config.js` at the root of the project and add the following contents:

```js
const autoprefixer = require("autoprefixer");

module.exports = {
  plugins: [
    {
      resolve: `gatsby-plugin-postcss-sass`,
      options: {
        postCssPlugins: [
          autoprefixer({
            browsers: ["last 2 versions"]
          })
        ],
        precision: 8
      }
    }
  ]
};
```

## CSS Modules

Gtasby comes with CSS modules support out of the box. The only requirement is to name all sass files as `filename.module.scss`.

## Site metadata

Site metadata is added in the `gatsby-config.js` file:

```js
const autoprefixer = require("autoprefixer");

module.exports = {
  siteMetadata: {
    title: `Title of the site`
  },
  plugins: [
    {
      resolve: `gatsby-plugin-postcss-sass`,
      options: {
        postCssPlugins: [
          autoprefixer({
            browsers: ["last 2 versions"]
          })
        ],
        precision: 8
      }
    }
  ]
};
```

To add the title to our final rendered `html`, we need to create a `layouts` folder inside the `src` folder and create a new `index.js` file inside, containing the following code:

```js
import React from "react";
import PropTypes from "prop-types";
import Helmet from "react-helmet";

import "./index.scss";

const Layout = ({ children, data }) => (
  <div>
    <Helmet title={data.site.siteMetadata.title} />
    <div>{children()}</div>
  </div>
);

Layout.propTypes = {
  children: PropTypes.func
};

export default Layout;

export const query = graphql`
  query SiteTitleQuery {
    site {
      siteMetadata {
        title
      }
    }
  }
`;
```

We also need to install `react-helmet`:

```bash
npm install --save react-helmet gatsby-plugin-react-helmet
```

Additional site metadata can be easily created and queried (note the use of the helmet plugin):

gatsby-config.js

```js
const autoprefixer = require("autoprefixer");

module.exports = {
  siteMetadata: {
    title: `Title of the site`,
    desc: `This is the description`
  },
  plugins: [
    `gatsby-plugin-react-helmet`,
    {
      resolve: `gatsby-plugin-postcss-sass`,
      options: {
        postCssPlugins: [
          autoprefixer({
            browsers: ["last 2 versions"]
          })
        ],
        precision: 8
      }
    }
  ]
};
```

src/layouts/index.js

```js
import React from "react";
import PropTypes from "prop-types";
import Helmet from "react-helmet";

import "./index.scss";

const Layout = ({ children, data }) => (
  <div>
    <Helmet title={data.site.siteMetadata.title} />
    <div>{data.site.siteMetadata.desc}</div>
    <div>{children()}</div>
  </div>
);

Layout.propTypes = {
  children: PropTypes.func
};

export default Layout;

export const query = graphql`
  query SiteTitleQuery {
    site {
      siteMetadata {
        title
        desc
      }
    }
  }
`;
```

## ESLint and Prettier

First, we need to instal ESLint and the react-app ESLint configuration (used in CRA):

```bash
npm install --save-dev eslint-config-react-app babel-eslint@^7.2.3 eslint@^4.1.1 eslint-plugin-flowtype@^2.34.1 eslint-plugin-import@^2.6.0 eslint-plugin-jsx-a11y@^5.1.1 eslint-plugin-react@^7.1.0
```

For Prettier support we need the following packages:

```bash
npm install --save-dev prettier eslint-config-prettier eslint-plugin-prettier
```

Then, we need to create a .eslintrc file at the root of the project with the following contents:

```js
{
  "extends": ["react-app", "plugin:prettier/recommended"]
}
```

And also a .prettierrc file:

```js
{
  "arrowParens": "avoid",
  "bracketSpacing": true,
  "jsxBracketSameLine": false,
  "parser": "babylon",
  "printWidth": 100,
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "all",
  "useTabs": false
}
```
