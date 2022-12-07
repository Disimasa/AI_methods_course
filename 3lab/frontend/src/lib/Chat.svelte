<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Comfortaa&display=swap" rel="stylesheet">

<script>
import Icon from "./Icon.svelte";
import Message from "./Message.svelte";
import { afterUpdate, tick } from 'svelte';

let currentText = ''
let messages = [
    { text: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aenean nec enim dignissim, pretium magna sit amet, sollicitudin erat. Integer rutrum maximus orci nec commodo. Sed et nisi id nulla faucibus aliquam id ut felis. Etiam at finibus nisl. Nulla at tellus id erat facilisis fermentum. Sed vulputate auctor est, et feugiat turpis ultricies vel.', isMine: true },
    { text: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aenean nec enim dignissim, pretium magna sit amet, sollicitudin erat. Integer rutrum maximus orci nec commodo. Sed et nisi id nulla faucibus aliquam id ut felis. Etiam at finibus nisl. Nulla at tellus id erat facilisis fermentum. Sed vulputate auctor est, et feugiat turpis ultricies vel.', isMine: false }
]
let messagesWrapperObj
let loading = false

afterUpdate(() => {
    console.log("afterUpdate");
    if(messages) scrollToBottom(messagesWrapperObj);
});

$: if (messages) {
    scrollToBottom(messagesWrapperObj)
}

function sendMessage() {
    if (!loading) {
        messages = [...messages, {text: currentText, isMine: true}]
        currentText = ''
    }

}

const scrollToBottom = async (node) => {
    console.log('here')
    node.scroll({ top: node.scrollHeight + 200, behavior: 'smooth' });
};

</script>

<div id="chat-window">
    <div id="chat-header">
        <Icon src="src/assets/robot.png" size="40"/>
        <p>Финансовый помощник</p>
    </div>
    <div id="messages-wrapper" bind:this={messagesWrapperObj}>
        {#each messages as message}
            <Message text={message.text} isMine={message.isMine} />
        {/each}
        {#if loading}
            <Message text="..." isMine={false}/>
        {/if}
    </div>
    <div id="chat-input-wrapper">
        <input type="text" bind:value={currentText}/>
        <button on:click={sendMessage}>
            Отправить
        </button>
    </div>
</div>

<style>
    #chat-window {
        display: grid;
        grid-template-rows: 60px auto 60px;
        flex-direction: column;
        height: 80vh;
        max-width: 800px;
        border: 1px solid rgba(0,0,0,.125);
        border-radius: 20px;
        font-family: 'Comfortaa', cursive;
    }

    #chat-header {
        display: flex;
        align-items: center;
        justify-content: start;
        border-bottom: 1px solid rgba(0,0,0,.125);
        border-top-left-radius: 20px;
        border-top-right-radius: 20px;
        background-color: #f7f7f7;
        padding: 0 20px;
    }

    #chat-header p {
        margin: 0 0 0 20px;
        font-size: 1.5rem;
    }

    #messages-wrapper {
        overflow-y: auto;

    }

    #messages-wrapper::-webkit-scrollbar { width: 0; }

    #chat-input-wrapper {
        display: flex;
        justify-content: center;
        align-items: center;
        padding: 20px;
        border-top: 1px solid rgba(0,0,0,.125);
        border-bottom-left-radius: 20px;
        border-bottom-right-radius: 20px;
        background-color: #f7f7f7;
    }

    #chat-input-wrapper input {
        width:100%;
        height: 30px;
        border: 1px solid rgba(0,0,0,.125);
        border-radius: 10px;
        margin-right: 10px;
        padding: 0 10px;
    }

    #chat-input-wrapper button {
        /*height: 30px;*/
    }
</style>



