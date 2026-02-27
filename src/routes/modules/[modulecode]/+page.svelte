<script>
    export let params;
    let moduleCode = parseInt(params.modulecode);

    // // JSON array of module data
    // const modules = [
    //     {
    //         "id": 2027,
    //         "title": "H2027 Front End Development",
    //         "details": "learn about Svelte and SvelteKit for dynamic website development"
    //     },
    //     {
    //         "id": 2029,
    //         "title": "COMP H2029 Database Fundamentals",
    //         "details": "learn about H2029 Database Fundamentals"
    //     },
    //     {
    //         "id": 2031,
    //         "title": "COMP H2031 Object-Oriented Programming",
    //         "details": "learn OO Java programming stuff"
    //     }
    // ];

    import modules from '$lib/data/modules.json';


    // use array find() method to seach for object matching module code from URL
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