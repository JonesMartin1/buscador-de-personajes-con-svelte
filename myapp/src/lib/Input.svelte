<script>
    import {each} from "svelte/internal"
    let value = ""
    let response = []
    let loading = false

    const handleInput = (event) => 
        value = event.target.value

    $: if(loading==true){
        fetch (`https://gateway.marvel.com:443/v1/public/characters?ts=1&name=${value}&apikey=bfaa7b237bb944372278afba5cb59bad&hash=69bb2ecd29ffccb8c99ff39fff85fb16`)
        .then(res => res.json())
        .then(JSON => {
            response = JSON.data.results || []
            console.log(response)
        })
        loading = false
    }

    function buscar(){
        loading = true
    }
    
</script>

<input  
    placeholder="Buscar personaje de Marvel"
    value = {value}
    on:input={handleInput}

/>


<button on:click={buscar}>
    Buscar

</button>

{#if loading}
    cargando....

{:else}
    {#if response.length > 0}
        {#each response as personaje}
        <div>
            {personaje.name}
        </div>
        <div>
            {personaje.description}
        </div>
            <article>
                <img alt = {personaje.description} src = {personaje.thumbnail.path}.{personaje.thumbnail.extension}/>
            </article>
        {/each}
        {:else}
            sin resultados
        {/if}
{/if}

<style>
    article{
        border: 1px solid #eee;
        border-radius: 4px;
        padding: 16px;
    }

</style>
