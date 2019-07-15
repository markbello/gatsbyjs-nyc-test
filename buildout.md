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
12. Create `components/logo.js`: add `gatsby-logo.png` to the repo and shortcut *ga-logo* for the entire file
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
14. Create `pages/feature-requests.js` using *ga-feature*

```
ga-feature needs building
```

15. Add `<Link />` to `pages/index.js`. Use *ga-link*
```js
<Link to={`/feature-requests`}>Feature Requests</Link>
```

...

deploy