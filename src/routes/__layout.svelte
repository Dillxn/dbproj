<script lang="ts">
	import { onMount } from 'svelte';

	let ready: boolean = false;
	let maximized: boolean = false;
	onMount(() => (ready = true));

	function minimize() {
		window.api.send('frame-minimize');
	}

	function maximize() {
		window.api.send('frame-maximize');
		maximized = true;
	}

	function restore() {
		window.api.send('frame-restore');
		maximized = false;
	}

	function exit() {
		window.close();
	}
</script>

<div id="frame" class:maximized>
	<div id="titlebar">
		<div id="dragarea" />
		<div class="buttons">
			<div class="minimize" on:click={minimize}>
				<i class="fa-solid fa-window-minimize" />
			</div>
			{#if maximized}
				<div class="restore" on:click={restore}>
					<i class="fa-solid fa-compress" />
				</div>
			{:else}
				<div class="maximize" on:click={maximize}>
					<i class="fa-solid fa-expand" />
				</div>
			{/if}
			<div class="exit" on:click={exit}>
				<i class="fa-solid fa-xmark" />
			</div>
		</div>
	</div>
	{#if ready}
		<slot />
	{/if}
</div>

<style>
	#frame {
		background-color: #222;
		color: white;
		border-radius: 10px;
		overflow: hidden;
		height: 100vh;
		width: 100vw;
		display: flex;
		flex-direction: column;
		align-items: stretch;
		user-select: none;
		transition: border-radius 0.1s ease;
	}
	#frame.maximized {
		border-radius: 0;
	}
	#titlebar {
		display: flex;
		height: 40px;
	}
	#titlebar #dragarea {
		-webkit-app-region: drag;
		position: relative;
		z-index: 100;
		flex: 1;
	}
	#titlebar .buttons {
		width: 200px;
		display: flex;
	}
	#titlebar .buttons > div {
		flex: 1;
		display: flex;
		align-items: center;
		justify-content: center;
		transition: all 0.18s ease;
	}
	#titlebar .buttons > div:hover {
		background-color: rgba(255, 255, 255, 0.1);
	}
	#titlebar .buttons > div:active {
		background-color: rgba(255, 255, 255, 0.15);
	}
	#titlebar .buttons > .exit:hover {
		background-color: rgba(255, 0, 0, 0.4);
	}
	#titlebar .buttons > .exit:active {
		background-color: rgba(255, 0, 0, 0.7);
	}
</style>
