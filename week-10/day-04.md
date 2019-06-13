---
description: It's the progressive web Old Sport
---

# Day 04

What we covered today:

* [Basic Sorting Solutions](https://github.com/wofockham/sei-31/tree/master/13-advanced/basic-sorts)
* [Gatsby](https://www.gatsbyjs.org/)
  * [Hello-world](https://github.com/wofockham/sei-31/tree/master/12-react/gatsby/hello-world)
  * [CSS-In-JS](https://github.com/wofockham/sei-31/tree/master/12-react/gatsby/css-in-js)
  * [Typography](https://github.com/wofockham/sei-31/tree/master/12-react/gatsby/typography)
* [Surge](https://surge.sh/)

Warmup

* [Rails Ajax Proxy](https://github.com/Yiannimoustakas/sei31-homework/tree/master/warmups/week10/day04_ajax_proxy)

## Gatsby

#### What is Gatsby?

Gatsby is a [React](https://reactjs.org/docs/getting-started.html)-based, [GraphQL](https://graphql.org/learn/) powered, [static site generator](https://www.netlify.com/blog/2017/05/25/top-ten-static-site-generators-of-2017/). What does that even mean?  Well, it weaves together the best parts of React, [webpack](https://webpack.js.org/concepts/), [react-router](https://reacttraining.com/react-router/core/guides/philosophy), GraphQL, and other front-end tools in to one very enjoyable developer experience. 

**Official Twitter Page**: [@gatsbyjs](https://twitter.com/gatsbyjs?ref_src=twsrc%5Egoogle%7Ctwcamp%5Eserp%7Ctwgr%5Eauthor)

**Official Github Page**: [gatsbyjs](https://github.com/gatsbyjs/gatsby)

**Installfest:**

```bash
npm install -g gatsby
```

Start a new project:

```bash
gatsby new hello-world https://github.com/gatsbyjs/gatsby-starter-hello-world
```

To start the development server:

```bash
gatsby develop
```

## Surge

#### What is Surge sh?

Shipping web projects should be fast, easy, and low risk. _Surge_ is static web publishing for Front-End Developers, right from the CLI.

To deploy to Surge:

First build your static html pages in your public folder. To do this run:

```bash
npm run build
```

Make sure your html files are present in your public folder:

```bash
tree public/
```

To deploy simply type:

```bash
surge public
```

## Further Reading

* [What is Gatsby JS?](https://www.mediacurrent.com/what-is-gatsby.js/)
* [CSS-In-JS.org](https://cssinjs.org/?v=v10.0.0-alpha.16)
* [CSS-In-JS All You Need To Know](https://hackernoon.com/all-you-need-to-know-about-css-in-js-984a72d48ebc)
* [How to deploy CRAPP with Surge](https://medium.freecodecamp.org/surge-vs-github-pages-deploying-a-create-react-app-project-c0ecbf317089)

