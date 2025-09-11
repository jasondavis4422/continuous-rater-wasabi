<!-- this page displays a demo version of the rating box that doesn't store data -->

<script>
    import { createEventDispatcher } from 'svelte';
	import RatingBox from '../RatingBox.svelte';
    import { experiment } from '../utils.js';
    
    const dispatch = createEventDispatcher();
    
    export let time = 0;
    export let ratingType;
	let pathway = `${experiment}/demo`;
	let paused = false;
    let rating = 50.0;
    
    function handleEnd() {
		dispatch('finished');
    };
    
    function handleBack() {
		dispatch('back');
	}; 
</script>

<style>
	p {
		font-weight: normal;
		padding: none;
	}

	.container {
		display: flex;
		flex-direction: column;
		align-items: center;
		padding: 1em;
		margin: 0 auto !important;
		min-width: 400px !important;
		max-width: 1000px !important;
	}

    .next {
    background-color: lightblue;
	}

    .back {
        background-color: lightcoral;
    }

    .key-box {
        align-items: center;
        text-align: center;
        margin: 0 auto !important;
    }

    .keys {
        align-items: center;
        text-align: left;
        margin: 0 auto !important;
        width: 25%;
        padding: 2%;
        background-color: rgba(255, 255, 255, 0.4);
        border: 2px solid grey;
        border-radius: 2px;
        box-shadow: 2px 2px 8px rgba(0, 0, 0, 0.1);

    }

    h1 {
        text-align: center;
    }
</style>

<div>
	<h1>Demo</h1>
	<div class="container">
		<RatingBox 
			pathway={pathway}
			rating={rating}
			bind:time={time}
			paused={paused}
			ratingType={ratingType}
            ratingBoxUse='demo page'>
		</RatingBox>
	</div>
	<div class="key-box">
		<div class="keys">
			<p>As you watch the movie, please use the up and down key to rate how <strong> {ratingType} </strong> you the movie makes you feel. You&rsquo;ll rate your emotion by moving up or down along the sliding scale. Hold either arrow key to accelerate in that direction. Release key to reset acceleration.</p>
<p>Anchor points of the scale are <strong>&quot;Not at all&quot; </strong> to  <strong> &quot;Extremely&quot; </strong>, so please input your ratings within this range. For example, let's go back to the example of the character saying something funny during the movie. If you found it very funny, move your rating up towards the &quot;Extremely&quot; anchor point. If you did not find it very funny, move your rating down towards the &quot;Not at all&quot; anchor point.</p>
<p>Please make these ratings continuously, whenever you experience a change in your emotional state. </p>

		</div>
		<br>
		<button class="back" on:click={handleBack}>Back</button>
		<button class="next" on:click={handleEnd}>Next</button>
	</div>
</div>

