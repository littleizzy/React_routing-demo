1.  npm i react-router-dom

2.  Inside index.js, wraps <App> with <BrowserRouter>

    import { BrowserRouter } from "react-router-dom";

           ReactDOM.render(
             <BrowserRouter>
               <App />
             </BrowserRouter>
             document.getElementById("root")
           );

3.  <Route />
    Inside app.js, adds multiple <Route path="/" component={Home} /> tags. Both switch and exact solve the url matching issue(ex. home is always rendered at the bottom). Switch renders the first route that matches the url. Order from most specific -> most generic. It's a wrapper for the given component, which automatically injects 3 props (history/location/match) to the component

- Learn more about the props: https://reacttraining.com/react-router/core/guides/philosophy

        import { Route, Switch } from "react-router-dom";

        <Switch>
           <Route path="/products" component={Products} />
           <Route path="/posts" component={Posts} />
           <Route path="/admin" component={Dashboard} />
           <Route path="/" exact component={Home} />
         </Switch>

- If you want to pass additional props as well as including the 3 props. Change component to render and pass in a function

        <Route
          path="/products"
          render={props => <Products sortBy="newst" {...props} />}
        />

4.  SPAS

- SPAS: refers to single page applications like gmail. Only the changed content get rerendered, not a full reload. Why its super fast.
- bundle.js is the combination of all js code, it's downloaded everytime the page reloads.
- "<a href="url"/>" tags is going to fully reload everytime you clicks it. Replace with <Link />
- "<Link to="url"/>" tags prevent the web from fully reloading. Instead, it only changes the url, which triggers Route to load the component only

        import { Link } from "react-router-dom";
        <Link to="/">Home</Link>

5.  Routing parameters

            <Route path="/products/:id" component={ProductDetails} />
            {this.props.match.params.id}

    - Optional params, append ?, o.w it will be treated as required params. But generally speaking you should avoid optional params and use query string.
      <Route path="/posts/:year?/:month?" component={Posts} />

6.  Query String

    - npm i query-string
    - stored in Location.search props, and we use queryString to parse it. Result is in string format. If it's number/bool, need additional parsing

          search: "?sortBy=newest&approved=true"
          {approved: "true", sortBy: "newest"}

7.  Redirect

    - If none of the url matches, and it's not an exact / path, we redirect to /not-found route
    - Also used to redirect from one url to another

          import { Route, Switch, Redirect } from "react-router-dom";
          <Route path="/" exact component={Home} />
          <Redirect to="/not-found" />

          <Redirect from="/messages" to="/posts" />

8.  Programmatic Navigation

    - props.history.push(): add new address in the address history. Gobackable.
    - props.history.replace(): replaces current url with no history. Un-gobackable. Often used in login scenario, when a user logged in, you replace it. You don't push it, so when the user clicks back, it goes back to login page. Nonono.

          handleSave = () => {
            // Navigate to /products
            this.props.history.push("/products");
          };

9.  Nested Routing

    - <Route> is a regular component, you can put it anywhere. What it matters: if the current the url matches the path, we render the corresponding component at the tag location

            <Link to="/admin/users">Users</Link>
            <Route path="/admin/users" component={Users} />
