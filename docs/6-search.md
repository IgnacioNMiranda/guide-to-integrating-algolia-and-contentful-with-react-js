# Sending queries to the index

Now that we're able to fetch our posts, a really nice feature that Algolia provides us is performing search against our index blazingly fast! In order to get the most from our searches, we need to add some configurations into our index.

Go to your Algolia dashboard and then to your index settings in the `Configuration` tab. Then click on `Searchable attributes` and add `fields.title.en-US` and `fields.category.en-US`. With this we're telling Algolia to only search for matches within these 2 fields, improving the performance of the search.

Now let's modify our `App` component, let's add a search input and some states:

```jsx
function App() {
  const [searchValue, setSearchValue] = useState('')
  const onSearchChange = (e) => {
    setSearchValue(e.target.value)
  }

  const [posts, setPosts] = useState()
  useEffect(() => {
    const handler = async () => {
      const data = await getPosts(searchValue)
      if (data) setPosts(data)
      setPosts(data)
    }
    handler()
  }, [searchValue])

  return (
    <main>
      <h1 className="page-title">POSTS</h1>
      <input type="text" className="posts-search" placeholder="Type your search here" value={searchValue} onChange={onSearchChange} />
      <ul className="posts">
        {posts?.hits?.length ? posts.hits.map((hit) => (
          <li key={hit.objectID}>
            <Post post={hit} />
          </li>
        )) : <p className="no-results">No results!</p>}
      </ul>
    </main>
  )
}
```

Also add the styles for our input in the `App.css` file:

```css
.posts-search {
  margin-bottom: 1.5rem;
  width: 100%;
  box-sizing: border-box;
  padding: 10px 10px;
  border-radius: 3px;
  outline: 0;
  display: block;
  border: 1px solid rgb(42, 48, 57, 0.2);
}

```

Now we can type some query and update our results! We can search for words within the title of our posts or for categories. If I type `Developers`, the posts will be filtered and those with that category will show up.

@TODO: add image