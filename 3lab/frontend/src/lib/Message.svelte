<script>
    import Icon from "./Icon.svelte";

    export let text= ''
    export let isMine = true
</script>

<div class="message-wrapper {isMine ? 'my-message' : 'companion-message'}">
    <div class="logo-wrapper">
        <Icon src="{isMine ? 'src/assets/person.png' : 'src/assets/robot.png'}" size="40"/>
    </div>
    <div class="text-wrapper">
        <p>{text}</p>
    </div>
</div>

<style>
    .message-wrapper {
        display: flex;
        margin: 10px 0;
    }

    .my-message {
        flex-direction: row-reverse;
    }

    .companion-message {
        flex-direction: row;
    }

    .logo-wrapper {
        margin: 0 10px;
    }

    .text-wrapper {
        text-align: left;
        max-width: 80%;
        background-color: #dbd9d9;
        border-radius: 20px;
    }

    .text-wrapper p {
        margin: 10px 20px;
    }
</style>