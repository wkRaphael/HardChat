<script>
    /**
   * @type {HTMLInputElement}
   */
    let inputText;
    import { onMount } from 'svelte';

    let socket;
    let Messages=[
        {id: 0, who: 0, text: "my message"},
        {id: 1, who: 1, text: "his message"}
    ];

    onMount(() => {
        socket = new WebSocket('ws://localhost:3000');

        socket.addEventListener('message', (event) => {
            const receivedMessage = event;
            console.log('Received message:', receivedMessage);
            Messages = Messages.concat({id: 2, who: 1, text: JSON.parse(receivedMessage.data).message})
        });
    });

    
    //initial messages for testing, move this somewhere else later. who: 0 is self (green), who: 1 is other person (red)
    //send messages 
    function sendMessage() {
        if (inputText.value) {
            Messages = Messages.concat({id: 2, who: 0, text: inputText.value})
            doPost();
            inputText.value = "";
        }
        //adds message to list if its actually a message
    }
    /**
   * @param {{ key: any; }} e
   */
    function sendMessageKeypress(e) {
        if (e.key == "Enter") {
            sendMessage();
        }
    }
    async function doPost() {
        const res = await fetch('http://localhost:3000/post', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json', // Set the content type to JSON
            },
            body: JSON.stringify({ message: inputText.value }), // Convert the messageContent to JSON
        });
        const json = await res.json()
        let result = JSON.stringify(json)
        console.log(res.status);
    }
</script>
<!-- https://svelte.dev/docs/introduction -->
<body>
    <button on:click={sendMessage}>Send</button>
    <input bind:this={inputText} on:keydown={sendMessageKeypress}>
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