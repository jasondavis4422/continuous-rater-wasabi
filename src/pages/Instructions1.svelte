<!-- first instruction page -->
<script>
  // This is the Instructions page. It loops over the instructions array as a user reads and when click to the last page it notifies the main App.svelte component by dispatching a 'finished' event. When the last page of the instructions are reached the forward button turns into a "Take Quiz" button, but currently there is no quiz and it goes straight to the experiment
  import { createEventDispatcher } from 'svelte';
  export let ratingType;
  export let numOptions;

  // Add/remove items here to create more instructions pages
  const ratingInstruct = '<p>In this experiment, you will provide moment-by-moment ratings of your emotional state as you watch a movie and answer a series of follow-up questions. As you watch the movie, please use the up and down key to rate how <strong>' + ratingType + '</strong> you the movie makes you feel. <p> </p> Before watching this movie, you will be watching a 3-minute training video to familiarize yourself with the rating information and scale used in the experiment, then answering questions about the training session. Next, you will see a 30-minute movie split into two halves &ndash; these parts are each around 15 minutes &ndash; and answer comprehension questions about the content of the movie.  You&rsquo;ll rate your emotion by moving to up or down along the sliding scale. When you press the up key, the slider will move to higher, increasing the presence of your emotion. When you press the down key, the slider will move lower, decreasing the presence of your emotion.</p> <p>We&rsquo;d like you to make these ratings continuously, whenever you experience a change in your emotional state. For instance, imagine a character said something you found funny during the movie. You might experience humor for a short period of time until a certain point where the same level of humor is not present. Once your emotional state changes, please reflect this in your rating of the movie. We&rsquo;d like you to focus on, and rate, your momentary shifts in emotion, not just how you feel overall.</p> <p> <strong> Please follow instructions accordingly or else compensation will not be provided for your responses. There will be a training video to help explain and practice the instructions. </strong>'
  
  const instructions = [
    ratingInstruct
  ];

  const dispatch = createEventDispatcher();
  let currentPage = 0;

    function handleEnd() {
		dispatch('finished');
    };

  const backward = () => {
    currentPage -= 1;
    currentPage = Math.max(currentPage, 0);
  };
  const forward = () => {
    // Check if we're increasing the value of currentPage beyond the number of instructions, if so tell App.svelte we're ready to move to the quiz
    if (currentPage + 1 === instructions.length) {
      dispatch('finished');
    } else {
      currentPage += 1;
      currentPage = Math.min(currentPage, instructions.length - 1);
    }
  };
</script>

<style>
  .container {
    margin: 0 auto !important; 
    max-width: 800px;
    text-align: center;
  }

  .text-box {
        text-align: left;
		padding: 2%;
		background-color: rgba(255, 255, 255, 0.4);
		border: 2px solid grey;
		border-radius: 2px;
		box-shadow: 2px 2px 8px rgba(0, 0, 0, 0.1);   
	}


  
  .controls {
    min-width: 25%;
  }

  button {
    background-color: lightblue;
  } 
</style>
 

<div class="container">
  <h1>Instructions</h1>  
  <div class="text-box">
    <div class="content">
      {@html instructions[currentPage]}
    </div>
  </div>
  <br>
  <button class="button is-link controls" on:click={handleEnd}>
    {#if currentPage === instructions.length - 1}
      Go To Demo
    {:else}
      <span class="icon">
        Next
        <i class="fas fa-forward" />
      </span>
    {/if}
  </button>
</div>
