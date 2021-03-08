<script context="module">
    export async function loadUsers(query, fetch) {
        const res = await fetch(`https://jsonplaceholder.typicode.com/users${query}`);
        return await res.json();
    }

    export async function preload(page, session) {
        const {query} = page;
        let search = '';
        if (query.search) {
            search += `?q=${encodeURIComponent(query.search)}`;
        }
        const getUsers = await loadUsers(search, this.fetch);

        return {getUsers};
    }
</script>
<script>
    export let getUsers;
    //TODO recursive fields for deep value
    export let fields = {
        id: "ID",
        name: "Name",
        username: "Username",
        email: "Email",
        /*address: {
            label:"Address",
            fields:{
                street: "Street",
                suite: "Suite",
                city: "City",
                zipcode: "Zip Code",
                geo: {
                    label:"Geo",
                    fields:{
                        lat: "Latitude",
                        lng: "Longitude"
                    }
                }
            },
        },
        phone: "Phone",
        website: "Website",
        company: {
            label:"Company",
            fields: {
                name: "Name",
                catchPhrase: "Catch Phrase",
                bs: "Slogan"
            }
        }*/
    };
    //TODO could be reactive search using debounce
    $: users = getUsers;
    $: searchValue = null;
    async function searchUsers(){
        //debounce needed
        getUsers = await loadUsers(`?q=${encodeURIComponent(searchValue)}`, self.fetch);
    }

    $: if (searchValue) {
        searchUsers();
    }
    import UserTable from '../components/userTable.svelte';
</script>

<svelte:head>
    <title>Sapper project template</title>
</svelte:head>
<div class="mt-5 md:mt-0 md:col-span-2">
    <form action="#" method="get">
        <div class="shadow overflow-hidden sm:rounded-md">
            <div class="px-4 py-5 bg-white sm:p-6">
                <div class="grid grid-cols-6 gap-6">
                    <div class="col-span-6 sm:col-span-3">
                        <label for="search" class="block text-sm font-medium text-gray-700">Search Text</label>
                        <input
                                type="text"
                                name="search"
                                id="search"
                                autocomplete="Search"
                                class="mt-1 focus:ring-indigo-500 focus:border-indigo-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md"
                                bind:value={searchValue}
                        >
                    </div>
                </div>
            </div>
            <div class="px-4 py-3 bg-gray-50 text-right sm:px-6">
                <button type="submit" class="inline-flex justify-center py-2 px-4 border border-transparent shadow-sm text-sm font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
                    Search
                </button>
            </div>
        </div>
    </form>
</div>
<UserTable users="{users}" {fields}/>