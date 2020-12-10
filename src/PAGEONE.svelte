<!-- <script>

let query = (ref) => ref.orderBy('flavor').limit(3)

function nextPage(last) {
    query = (ref) => ref.orderBy('flavor').startAfter(last.flavor).limit(3);
}
</script>

<Collection path={'foods'} {query} let:data let:last>
    
    {#each data as food}
    {food.name}
    {/each}
    
    <button on:click={() => nextPage(last) }>Next</button>
    
    
</Collection> -->
<StorageRef {path} let:downloadURL let:ref meta let:metadata> 
  
    <img src={downloadURL} />

    <div slot="loading">
        Loading...
    </div>

    <div slot="fallback">
        Error
    </div>

</StorageRef>