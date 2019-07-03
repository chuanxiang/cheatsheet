
## Fetch
```
fetch(url)
  .then((response)=>response.json())
  .then((data)=>xxxx)
  .catch((error)=>{
    xxxx
  })
```

## destructring with default value?
const {
  movies = []
} = this.props


