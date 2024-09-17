# Angular New Features

## Server side rendering (SSR)

Create a new angular project with SSR/SSG support enabled

```bash
ng new my-ssr-app --ssr
```

Create 3 different routes in the app

```bash
/ - Displays a basic welcome message
/todos - Displays a list of todos. Use a service to fetch the todos from https://jsonplaceholder.typicode.com/todos)
/todos/:id - Displays the details of a todo. Use a service to fetch the todo from https://jsonplaceholder.typicode.com/todos/:id
```

This should be a very basic app with no styling. The focus is on the SSR part. Use the new @if and @for directives in the HTML to render the todos in the list. You should not use the old *ngIf and *ngFor directives since this is a lab on the new features of Angular.

## SSR and Network traffic

Open the network tab in the browser and navigate to the /todos route. Since you have SSR enabled, you should not see any network traffic to fetch the todos from the typicode API. The todos should be fetched on the server and rendered in the HTML.

You can disable SSR and prerendering in the angular.json file to see the difference. Find the part that looks like this:

```json
"prerender": true,
"ssr": {
    "entry": "server.ts"
}
```

and change it to;

```json
"prerender": false,
"ssr": false
```

When you rerun the application it should now fetch the todos from the API on the client side (Check this in the network tab). You should understand the difference between SSR and CSR. If you do not, please ask the lecturer to explain why there is a difference.

## SSG or prerendering

Change the angular.json file back to the original settings and run the application again. Now we are going to generate a static site from the application. Run the following command:

```bash
ng build
```

The output should contain something like this:

```bash
Prerendered 2 static routes.
Application bundle generation complete. [3.532 seconds]
```

This will generate a dist folder with the static site. It will contain a folder named browser with the client side code and a folder named server with the server side code. If you run a http-server inside the dist/browser folder you should see the application running as a static site. http-server comes preinstalled in this devcontainer.

```bash
http-server
```

Note that the ng build command will not generate the static pages for the /todos/:id route. This is because it has a dynamic parameter. If you want to prerender those pages you should change the configuration in the angular.json file to the following:

```json
"prerender": {
    "routesFile": "routes.txt"
},
"ssr": {
    "entry": "server.ts"
}
```

Create a file named routes.txt in the root of the project with the following content:

```
/
/todos
/todos/1
/todos/2
/todos/3
...
```

Now when you run the ng build command it will prerender the pages in the routes.txt file. You can now run the http-server command in the dist/browser folder to see the prerendered pages.

## Signals

Rework your application to use a todos signal instead of a todos field in the component. Make two computed signals which provide the amount of completed and uncompleted todos. Provide a way of updating the completed status of a todo by clicking a checkbox, make sure the signal is updated when the status changes. This should just be local and not persisted to the server. 