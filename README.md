Gatsby Plugin Remark Collection
---

If you use multiple `gatsby-source-filesystem` definitions to load multiple
directories of content, how can you request just one directory in Gatsby? You
can use a RegEx on the file system path. Ugly. This plugin adds a "field" called
"colllection" which simply copies the value from the `File` node to the
`MarkdownRemark` node.

Example `gatsby-config.js`:

```javascript
{
  resolve: "gatsby-source-filesystem",
  options: {
    name: "posts",
    path: `${__dirname}/content/posts`
  }
},
{
  resolve: "gatsby-source-filesystem",
  options: {
    name: "tutorials",
    path: `${__dirname}/content/tutorials`
  }
},
```

Now you can query for these specifically like so:

```graphql
posts: allMarkdownRemark(
  filter: { fields: { collection: { eq: "posts" } } }
  sort: { fields: [frontmatter___date], order: DESC }
) {
  edges {
    node {
      id
      frontmatter {
        title
        path
        tags
      }
    }
  }
}
tutorials: allMarkdownRemark(
  filter: { fields: { collection: { eq: "tutorials" } } }
) {
  edges {
    node {
      id
      frontmatter {
        title
        path
      }
    }
  }
}
```

The field `collection` was added by this plugin.
