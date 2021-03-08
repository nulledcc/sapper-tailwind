<script>
    import { onMount, afterUpdate } from 'svelte';
    export let users;
    export let fields;
    // TODO recursive fields if needed
    // const allowedFields = {};
    // function recursiveFields(fields){
    //
    // }
    // //recursive build fields


    onMount(() => {
        console.log("mount users:", users);
    })

    afterUpdate(() => {
        console.log("after update users:", users);
    })
</script>

<!-- This example requires Tailwind CSS v2.0+ -->
<div class="flex flex-col">
    <div class="-my-2 overflow-x-auto sm:-mx-6 lg:-mx-8">
        <div class="py-2 align-middle inline-block min-w-full sm:px-6 lg:px-8">
            <div class="shadow overflow-hidden border-b border-gray-200 sm:rounded-lg">
                <table class="min-w-full divide-y divide-gray-200">
                    <thead class="bg-gray-50">
                    <tr>
                        {#each Object.keys(fields) as field, i}
                            <th scope="col"
                                class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                                {fields[field].label || fields[field]}
                            </th>
                        {/each}
                    </tr>
                    </thead>
                    <tbody class="bg-white divide-y divide-gray-200">
                    {#each users as user (user.id)}
                        <tr>
                            {#each Object.keys(fields) as field, i}
                                <td class="px-6 py-4 whitespace-nowrap">
                                    <div class="flex items-center">
                                        {user[field]}
                                    </div>
                                </td>
                            {/each}
                        </tr>
                    {/each}
                    <!-- More items... -->
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</div>
