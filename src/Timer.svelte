<script>
    import ProgressBar from './ProgressBar.svelte';
    import { createEventDispatcher } from 'svelte';

    const totalSeconds = 20;
    let secondsLeft = totalSeconds;
    let isRunning = false;
    $: progress = ((totalSeconds - secondsLeft) / totalSeconds) * 100;
    const dispatch = createEventDispatcher();

    function startTimer() {
        isRunning = true;
    
        const timer = setInterval(() => {
            secondsLeft -= 1;
            if(secondsLeft == 0){
                secondsLeft = totalSeconds;
                clearInterval(timer);
                isRunning = false;
                dispatch('end', 'end timer');
            }
        }, 1000)
    }
</script>

<style>
    h2{
        margin: 0;
    }
    
    .start{
        background-color: rgb(154, 73, 73);
        width: 100%;
        margin: 10px 0;
    }

    .start[disabled]{
        background-color: rgba(194, 194, 194, 1);
        cursor: not-allowed;
    }
</style>

<div bp="grid">
    <h2 bp="offset-5@md 4@md 12@sm">
        Seconds Left: {secondsLeft}
    </h2>
</div>
<ProgressBar {progress}></ProgressBar>

<div bp="grid">
    <button
        disabled={isRunning}
        bp="offset-5@md 4@md 12@sm" 
        class="start "
        on:click={startTimer}>
        Start
    </button>
</div>