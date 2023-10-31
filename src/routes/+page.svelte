<script>
    let inputText; //this works but vscode no likey, fix it if you can figure it out
    let Messages=[
        {id: 0, who: 0, text: "my message"},
        {id: 1, who: 1, text: "his message"}
    ];
    //initial messages for testing, move this somewhere else later. who: 0 is self (green), who: 1 is other person (red)
    //send messages 
    function sendMessage() {
        if (inputText.value) {
            Messages = Messages.concat({id: 2, who: 0, text: inputText.value})
        }
        //adds message to list if its actually a message
    }
</script>
<!-- https://svelte.dev/docs/introduction -->
<body>
    <button on:click={sendMessage}>Send</button>
    <input bind:this={inputText}>
    <!-- cool svelte shit, if you cant figure out what it does you are a dumbass -->
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