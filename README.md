Back to FEDev main <<<< https://github.com/dr-matt-smith/2026-FDEev-Front-End-Dev-start-here

JSON-driven website: 
[Step 1](https://github.com/dr-matt-smith/FEDev-JSON-data-driven-website-step-1)
|
[Step 2](https://github.com/dr-matt-smith/FEDev-JSON-data-driven-website-step-2)
|
[Step 3](https://github.com/dr-matt-smith/FEDev-JSON-data-driven-website-step-3)
|
Step 4
|
[Step 5](https://github.com/dr-matt-smith/FEDev-JSON-data-driven-website-step-5)
|
[Step 6](https://github.com/dr-matt-smith/FEDev-JSON-data-driven-website-step-6)
|
[Step 7](https://github.com/dr-matt-smith/FEDev-JSON-data-driven-website-step-7)

# FEDev-JSON-data-driven-website-step-4

Step-by-step development of a module details website. Routing in the form `/module/2037`


## 1: Have an JSON array of module details, and search to find module matching URL module code

Let's generalise our `/routes/modules/[modulecode]/+page.svelte` code, to allow for data about many modules, that might come form some data source such as a Database or API.

So let's encode the data for our three modules as objects in a JSON array
- note, since the module ID is an integer (typical for database IDs) we'll represent this as an integer in our JSON

```javascript
const modules = [
    {
        "id": 2027,
        "title": "H2027 Front End Development",
        "details": "learn about Svelte and SvelteKit for dynamic website development"
    },
    {
        "id": 2029,
        "title": "COMP H2029 Database Fundamentals",
        "details": "learn about H2029 Database Fundamentals"
    },
    {
        "id": 2031,
        "title": "COMP H2031 Object-Oriented Programming",
        "details": "learn OO Java programming stuff"
    }
]
```

We can use the JavaScript array `find()` method to attempt to locate a module object whose code matches the module code from the HTTP Request parameters
- if no such module can be found in the array, we can populate a `module` variable with error values

```javascript
// use array find() method to search for object matching module code from URL
let module = modules.find((module) => module.id === moduleCode);

// if we didn't find a module, then populate with error details
if (!module) {
    module = {
        id: moduleCode,
        title: "ERROR",
        details: "unknown module code"
    };
}
```

So the full listing for `/routes/modules/[modulecode]/+page.svelte`  is now as follows
- note, since the module code is an integer in the JSON, we need to use JavaScript functon `parseInt()` to get the integer version of the text from the HTTP Request URL
  - `let moduleCode = parseInt(params.modulecode)`

```html
<script>
    // export let params;  << $app/state is now the recommended method to access page parameters
    import { page } from '$app/state';
    let params = page.params;
    
    let moduleCode = parseInt(params.modulecode);
      
    // JSON array of module data
    const modules = [
        {
            "id": 2027,
            "title": "H2027 Front End Development",
            "details": "learn about Svelte and SvelteKit for dynamic website development"
        },
        {
            "id": 2029,
            "title": "COMP H2029 Database Fundamentals",
            "details": "learn about H2029 Database Fundamentals"
        },
        {
            "id": 2031,
            "title": "COMP H2031 Object-Oriented Programming",
            "details": "learn OO Java programming stuff"
        }
    ];

    // use array find() method to search for object matching module code from URL
    let module = modules.find((module) => module.id === moduleCode);

    // if we didn't find a module, then populate with error details
    if (!module) {
        module = {
            id: moduleCode,
            title: "ERROR",
            details: "unknown module code"
        };
    }
</script>

<h1>
    Details of module with code = {module.id}
</h1>

<h3>{module.title}</h3>
<p>
    {module.details}
</p>
```

## 2: Move our JSON data array into a different script

Let's create a new JavaScript file declaring the JSON module object array.

Create directory `/lib/data`, and create file `/lib/data/modules.json` containing our JSON array declaration:

`/lib/data/modules.json`
```javascript
[
    {
        "id": 2027,
        "title": "H2027 Front End Development",
        "details": "learn about Svelte and SvelteKit for dynamic website development"
    },
    {
        "id": 2029,
        "title": "COMP H2029 Database Fundamentals",
        "details": "learn about H2029 Database Fundamentals"
    },
    {
        "id": 2031,
        "title": "COMP H2031 Object-Oriented Programming",
        "details": "learn OO Java programming stuff"
    }
]
```

In any Svelte script we can access this data with an `import` statement:
```javascript
    import modules from '$lib/data/modules.json';
```

So we can simplify our Svelte page  `/routes/modules/[modulecode]/+page.svelte`  to the following:

```html
<script>
    // export let params;  << $app/state is now the recommended method to access page parameters
    import { page } from '$app/state';
    let params = page.params;
  
    let moduleCode = parseInt(params.modulecode);

    import modules from '$lib/data/modules.json';
    
    // use array find() method to search for object matching module code from URL
    let module = modules.find((module) => module.id === moduleCode);

    // if we didn't find a module, then populate with error details
    if (!module) {
        module = {
            id: moduleCode,
            title: "ERROR",
            details: "unknown module code"
        };
    }
</script>

<h1>
    Details of module with code = {module.id}
</h1>

<h3>{module.title}</h3>
<p>
    {module.details}
</p>
```
