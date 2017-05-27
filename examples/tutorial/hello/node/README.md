# Tutorial 1: NodeJS Function w/ Input (3 minutes)

This example will show you how to test and deploy Node code to Oracle Functions. It will also demonstrate passing data in through stdin.

### First, run the following commands:

```sh
# Initialize your function creating a func.yaml file
fn init <DOCKERHUB_USERNAME>/hello

# Test your function. 
# This will run inside a container exactly how it will on the server. It will also install and vendor dependencies from Gemfile
fn run

# Now try with an input
cat hello.payload.json | fn run

# Deploy your functions to the Oracle Functions server (default localhost:8080)
# This will create a route to your function as well
fn deploy myapp
```
### Now call your function:

```sh
curl http://localhost:8080/r/myapp/hello
```

Or call from a browser: [http://localhost:8080/r/myapp/hello](http://localhost:8080/r/myapp/hello)

And now with the JSON input:

```sh
curl -H "Content-Type: application/json" -X POST -d @hello.payload.json http://localhost:8080/r/myapp/hello
```

That's it!

### Note on Dependencies

Create a [package.json](https://docs.npmjs.com/getting-started/using-a-package.json) file in your functions directory.

Run:

```sh
docker run --rm -v "$PWD":/function -w /function funcy/node:dev npm install
```

Then everything should work.

For example, using the `package.json` file in this directory which includes the [request](https://www.npmjs.com/package/request) package, you can add this to func.js and it will work:

```js
var request = require('request');
request('http://www.google.com', function (error, response, body) {
  if (!error && response.statusCode == 200) {
    console.log(body) // Show the HTML for the Google homepage.
  }
})
```


# In Review

1. We piped JSON data into the function at the command line
    ```sh
    cat hello.payload.json | fn run
    ```

2. We received our function input through **stdin**
    ```node
    obj = JSON.parse(fs.readFileSync('/dev/stdin').toString())
    ```

3. We wrote our output to **stdout**
    ```node
    console.log
    ```

4. We sent **stderr** to the server logs
    ```node
    console.error
    ```


# Next Up
## [Tutorial 2: Input Parameters](examples/tutorial/params)


