<script>
	import { onMount } from 'svelte';

	import {
		UP_ARROW,
		DOWN_ARROW,
		LEFT_ARROW,
		RIGHT_ARROW,
		W,
		A,
		S,
		D,
	} from './lib/keys';

	let stratagemIndex = 0;
	let keyIndex = 0;

	const currentStratagem = {
		name: 'Testgem',
		icon: 'test',
		totalKeys: 4,
		keys: ['left', 'right', 'up', 'down'],
	};

	onMount(() => {
		document.addEventListener('keydown', handleKeyInput);
	});

	function handleKeyInput(e) {
		if (e.code === UP_ARROW || e.code === W) {
			checkForValidInput('up');
		} else if (e.code === DOWN_ARROW || e.code === S) {
			checkForValidInput('down');
		} else if (e.code === LEFT_ARROW || e.code === A) {
			checkForValidInput('left');
		} else if (e.code === RIGHT_ARROW || e.code === D) {
			checkForValidInput('right');
		}
	}

	function checkForValidInput(key) {
		if (key === currentStratagem.keys[keyIndex]) {
			if (keyIndex + 1 >= currentStratagem.totalKeys) {
				toggleKeyStatusColor(keyIndex, 'reset');
				keyIndex = 0;
				stratagemIndex++;
				return;
			}

			toggleKeyStatusColor(keyIndex, 'correct');
			keyIndex++;
			return;
		}

		toggleKeyStatusColor(keyIndex, 'reset');
		keyIndex = 0;
	}

	function toggleKeyStatusColor(index, status) {
		const key = document.getElementById(`key-${index}`);
		if (status === 'correct') {
			key.classList.add('correct');
		} else {
			for (let k = 0; k < currentStratagem.totalKeys; k++) {
				const key = document.getElementById(`key-${k}`);
				key.classList.remove('correct');
			}
		}
	}
</script>

<main>
	<h1>{stratagemIndex + 1}</h1>
	<div class="keys">
		{#each currentStratagem.keys as key, index}
			<span id={`key-${index}`}>{key}</span>
		{/each}
	</div>
</main>

<style>
	main {
		padding: 1rem;
		display: flex;
		flex-direction: column;
		gap: 1rem;
	}

	.keys {
		display: flex;
		gap: 1rem;
	}
	span {
		padding: 0.4rem 0.6rem;
		border: 1px solid black;
	}
</style>
