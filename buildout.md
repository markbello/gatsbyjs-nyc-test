# GatsbyJS NYC Meetup, Site Buildout Presentation

STEPS

1. Demo (Gatsby Starter Default)[https://www.gatsbyjs.org/starters/gatsbyjs/gatsby-starter-default/]
2. Scaffold site: gatsby new gatsbyjs-nyc https://github.com/gatsbyjs/gatsby-starter-default
3. Point to GatsbyJS NYC repo

```
git remote add origin https://github.com/markbello/gatsbyjs-nyc.git
git push -u origin master
```

4. Push up initial commit
5. Checkout to `feature/splash`
6. Open in VSCode
6a. Run `gatsby develop`
7. Give walkthrough of file structure
    - Pages Folder: uses Guess.js under the hood, which combines machine learning and Google analytics with route-based code-splitting, as well as smart pre-fetching. Vanilla React does not handle this out of the box.
    - Components Folder: contains shared components to be used as partials in pages. 
    - React Helmet: populates the `<head />` tag in HTML using a GraphQL call from `gatsby-config.js`
    - Gatsby Core Files
      - `gatsby-config.js` contains plugins and metadata, which can be queried using GraphQL calls throughout your app
      - `gatsby-browser.js` has a browser related API, and can also directly import global css
      - `gatsby-node.js` can handle dynamic routing
      - `gatsby-ssr.js` is pretty self-explanatory but beyond the scope of the presentation
8. Change the metadata inside `gatsby-config.js` *(shortcut ga-meta)*
	```
    title: `GatsbyJS NYC Meetup`,
    description: `Our community is a welcoming space where any Gatsby, React, Javascript, JAMStack related question is fair game, be it beginner, intermediate, or advanced. We don’t have all the answers, but we can find them as a team.`,
    author: `GatsbyJS NYC Meetup`,
  ```
8a. Restart dev server
9. Change the content in `pages/index.js` *(shortcut ga-index)*
  ```js
  const IndexPage = () => (
  <Layout>
    <SEO title="Home - Gatsby NYC" />
    <h1>Gatsby NYC Meetup</h1>
    <p>Connect with modern web technology community members. Gatsby is an amazing website framework (Open Source) that builds performance into every website by leveraging the latest web tech: React, GraphQL, Guess.js and modern Javascript. Meet and mingle with the Gatsby community as you learn from speakers about the future of website technologies.</p>
    <p>Our community is a welcoming space where any Gatsby, React, Javascript, JAMStack related question is fair game, be it beginner, intermediate, or advanced. We don’t have all the answers, but we can find them as a team.</p>
    <div style={{ maxWidth: `300px`, marginBottom: `1.45rem` }}>
      <Image />
    </div>
  </Layout>
  )
  ```
10. Introduce `gatsby-image` and `gatsby-plugin-sharp`: `gatsby-plugin-sharp` provides image optimization including lazy loading and dynamic image resizing, and is added through `gatsby-config.js`. `gatsby-image` provides a React `<Img />` component leveraging `gatsby-plugin-sharp`, which replaces the `<img />` tag

  ## Using gatsby-image
  - Import gatsby-image and use it in place of the built-in img
  - Write a GraphQL query using one of the included GraphQL “fragments” which specify the fields needed by gatsby-image.

  ## Features of gatsby-plugin-sharp
  - Resize large images to the size needed by your design
  - Generate multiple smaller images so smartphones and tablets don’t download desktop-sized images
  - Strip all unnecessary metadata and optimize JPEG and PNG compression
  - Efficiently lazy load images to speed initial page load and save bandwidth
  - Use the “blur-up” technique or a “traced placeholder” SVG to show a preview of the image while it loads
  - Hold the image position so your page doesn’t jump while images load

  ## Referencing local images
  - To use local images, GraphQL needs to know where to look. To do this, use the `gatsby-source-filesystem` plugin, and configure path resolution in `gatsby-config.js`
11. Walk through `components/image.js`
11a. Add `gatsby-logo.png` to the repo
12. Create `components/logo.js` with *ga-logo* for the entire file
  ```js
  import React from "react"
  import { useStaticQuery, graphql } from "gatsby"
  import Img from "gatsby-image"

  /*
  * This component is built using `gatsby-image` to automatically serve optimized
  * images with lazy loading and reduced file sizes. The image is loaded using a
  * `useStaticQuery`, which allows us to load the image from directly within this
  * component, rather than having to pass the image data down from pages.
  *
  * For more information, see the docs:
  * - `gatsby-image`: https://gatsby.dev/gatsby-image
  * - `useStaticQuery`: https://www.gatsbyjs.org/docs/use-static-query/
  */

  const Logo = () => {
    const data = useStaticQuery(graphql`
      query {
        placeholderImage: file(relativePath: { eq: "gatsby-logo.png" }) {
          childImageSharp {
            fluid(maxWidth: 300) {
              ...GatsbyImageSharpFluid
            }
          }
        }
      }
    `)

    return <Img fluid={data.placeholderImage.childImageSharp.fluid} />
  }

  export default Logo
  ```
13. Add logo to `components/header.js`. Replace entire file with *ga-header*
```js
  import { Link } from "gatsby"
import PropTypes from "prop-types"
import React from "react"
import Logo from "./logo";

const Header = ({ siteTitle }) => (
  <header
    style={{
      background: `white`,
      borderBottom: `1px solid #efefef`,
      marginBottom: `1.45rem`,
    }}
  >
    <div
      style={{
        margin: `0 auto`,
        maxWidth: 960,
        padding: `1.45rem 1.0875rem`,
      }}
    >
      <div style={{ margin: 0 }}>
        <Link
          to="/"
          style={{
            color: `rebeccapurple`,
            textDecoration: `none`,
          }}
        >
          <div style={{ maxWidth: '100px' }}>
            <Logo />
          </div>
        </Link>
      </div>
    </div>
  </header>
)

Header.propTypes = {
  siteTitle: PropTypes.string,
}

Header.defaultProps = {
  siteTitle: ``,
}

export default Header
```
13a. Delete `pages/page-2.js`
14. Create `pages/feature-requests.md` using *ga-feature*

```
## Feature Requests

- Add Styled Components
- Move to Typescript
- Add blog: when people present, they can add their presentation as a blog post
- Logo
- Add testing
- ...be creative

## Pages to Build

- Contributors Page
- Code of Conduct page (we abide by the (JSConf Code of Conduct)[https://jsconf.com/codeofconduct.html])
- ...be creative

## Presentation Ideas

- Implementing any of the requested features
- Gatsby plugins
- Gatsby themes
- Gatsby preview
- JAMstack: Netlify, Contentful, etc
- Testing
- Deep dive `gatsby-browser.js`
- Deep dive `gatsby-config.js`
- Deep dive `gatsby-node.js`
- Deep dive `gatsby-ssr.js`
- ...be creative
```
15. For Markdown transformation `npm install --save gatsby-transformer-remark`
16. Add `gatsby-transformer-remark` and new src path resolution to `gatsby-config.js` with *ga-config*
```js
module.exports = {
  siteMetadata: {
    title: `Gatsby NYC Meetup`,
    description: `Our community is a welcoming space where any Gatsby, React, Javascript, JAMStack related question is fair game, be it beginner, intermediate, or advanced. We don’t have all the answers, but we can find them as a team.`,
    author: `Gatsby NYC Meetup`,
  },
  plugins: [
    `gatsby-plugin-react-helmet`,
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        name: `images`,
        path: `${__dirname}/src/images`,
      },
    },
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        name: `src`,
        path: `${__dirname}/src/`,
      },
    },
    `gatsby-transformer-remark`,
    `gatsby-transformer-sharp`,
    `gatsby-plugin-sharp`,
    {
      resolve: `gatsby-plugin-manifest`,
      options: {
        name: `gatsby-starter-default`,
        short_name: `starter`,
        start_url: `/`,
        background_color: `#663399`,
        theme_color: `#663399`,
        display: `minimal-ui`,
        icon: `src/images/gatsby-icon.png`, // This path is relative to the root of the site.
      },
    },
    // this (optional) plugin enables Progressive Web App + Offline functionality
    // To learn more, visit: https://gatsby.dev/offline
    // `gatsby-plugin-offline`,
  ],
}

```
17. Add shortcut *ga-node* to `gatby-node.js` to create pages from `.md` files
```js
const path = require(`path`)
const { createFilePath } = require(`gatsby-source-filesystem`)

exports.onCreateNode = ({ node, getNode, actions }) => {
  const { createNodeField } = actions
  if (node.internal.type === `MarkdownRemark`) {
    const slug = createFilePath({ node, getNode, basePath: `pages` })
    createNodeField({
      node,
      name: `slug`,
      value: slug,
    })
  }
}

exports.createPages = ({ graphql, actions }) => {  // **Note:** The graphql function call returns a Promise  // see: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise for more info
  return graphql(`{
    allMarkdownRemark {
      edges {
        node {
          fields {
            slug
          }
        }
      }
    }
  }`).then(result => {
      result.data.allMarkdownRemark.edges.forEach(({ node }) => {
        createPage({
          path: node.fields.slug,
          component: path.resolve(`./src/templates/blog-post.js`),
          context: { slug: node.fields.slug },
        })
      })
   })}
```
18. Create `src/templates/blog-post.jsx` with *ga-template*
```js
import React from "react"
import { graphql } from "gatsby"
import Layout from "../components/layout"

export default ({ data }) => {
	const post = data.markdownRemark
	
	return (
		<Layout>
			<div>
				<h1>{post.frontmatter.title}</h1>
			<div dangerouslySetInnerHTML={{ __html: post.html }} /></div>
		</Layout>
	)
}

export const query = graphql`query($slug: String!) {
	markdownRemark(fields: { slug: { eq: $slug } }) {
		html
		frontmatter {
			title
		}
	}
}`
```
19. Add `<Link />` to `pages/index.js`. Use *ga-link*
```js
<Link to={`/feature-requests`}>Feature Requests</Link>
```
20. Add/commit/push to Github
...

deploy