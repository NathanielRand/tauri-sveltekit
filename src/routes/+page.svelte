<script lang="ts">
    import { invoke } from '@tauri-apps/api/tauri';

    let message = '';
    let isConnected = false;
    let isJoined = false;

    async function connectToIRC() {
        try {
            await invoke('connect_irc');
            isConnected = true;
        } catch (err) {
            console.error('Error connecting to IRC:', err);
        }
    }

    async function joinChannel() {
        try {
            await invoke('join_channel');
            isJoined = true;
        } catch (err) {
            console.error('Error joining channel:', err);
        }
    }

    async function sendMessage() {
        try {
            await invoke('send_message', { message });
            message = '';
        } catch (err) {
            console.error('Error sending message:', err);
        }
    }

    async function startVoiceChat() {
        try {
            await invoke('start_voice_chat');
            // Handle voice chat UI
        } catch (err) {
            console.error('Error starting voice chat:', err);
        }
    }
</script>

<main class="container text-center">
    <h1>IRC Voice Chat</h1>

    {#if !isConnected}
        <button on:click={connectToIRC}>Connect to IRC</button>
    {:else if !isJoined}
        <p>Connecting to IRC channel...</p>
		<div class="loader"></div> <!-- This is where the spinner/loader will be displayed -->
    {:else}
        <input type="text" bind:value={message} placeholder="Enter message" />
        <button on:click={sendMessage}>Send</button>
        <button on:click={startVoiceChat}>Start Voice Chat</button>
    {/if}
</main>

<style>
	.container {
		padding: 20px;
	}
	.text-center {
		text-align: center;
	}
	.loader {
		border: 4px solid #f3f3f3; /* Light grey */
		border-top: 4px solid #ee2673; /* Blue */
		border-radius: 50%;
		width: 30px;
		height: 30px;
		animation: spin 1s linear infinite;
		margin: 20px auto; /* Adjust this to center the loader */
	}

	@keyframes spin {
		0% { transform: rotate(0deg); }
		100% { transform: rotate(360deg); }
	}
</style>