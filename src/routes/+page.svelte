<script>
    let inputText;
    //this works 
    let Messages=[
        {id: 0, who: 0, text: "my message"},
        {id: 1, who: 1, text: "his message"}
    ];
    function sendMessage() {
        Messages = Messages.concat({id: 2, who: 0, text: inputText.value})
    }
</script>
<!-- https://svelte.dev/docs/introduction -->
<body>
    <button on:click={sendMessage}>Send</button>
    <input bind:this={inputText}>
    {#each Messages as myMessage}
        {#if myMessage.who == 0}
            <p class="myMessages">{myMessage.text}</p>
        {:else if myMessage.who == 1}
            <p class="othersMessages">{myMessage.text}</p>
        {/if}
    {/each}
    
</body>
<style>
    .myMessages{
        color: green;
    }
    .othersMessages{
        color: red;
    }
</style>