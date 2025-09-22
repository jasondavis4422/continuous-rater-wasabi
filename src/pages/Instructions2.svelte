<!-- second instruction page -->

<script>
  // This is the Instructions page. It loops over the instructions array as a user reads and when click to the last page it notifies the main App.svelte component by dispatching a 'finished' event. When the last page of the instructions are reached the forward button turns into a "Take Quiz" button, but currently there is no quiz and it goes straight to the experiment
  import { createEventDispatcher } from 'svelte';
  export let ratingType;

  // Add/remove items here to create more instructions pages
  let description = '';
if (ratingType === 'aroused') {
  description = '<p>Aroused is defined as how energized or stimulated you feel in response to the movie. Examples of items that make one feel more aroused are fast, upbeat music, going on a rollercoaster, or interviewing for a job. Note that feeling aroused is not inherently characterized as good or bad. Rather, if you experience a change in how energized you are (regardless of if it is good or bad), then please change your rating accordingly. </p>'
}
 else if (ratingType === 'happy') {
  description = '<p>Happy is defined as a general level of enjoyment or contentment to the movie. Examples of items that may reflect happiness are hanging out with friends, engaging in enjoyable hobbies or pursuing a meaningful goal. If you experience a change in your level of contentment to the movie, please change your rating accordingly.</p>';
} else if (ratingType === 'calm') {
  description = '<p>Calm is defined as how passive or unenergized you feel (a lack of energy or stimulation) in response to the movie. Examples that make one feel calmer are gray, dreary weather, reading a book, or doing some meditation. Note that feeling calm is not inherently characterized as good or bad. Rather, if you feel more passive and low-energy in response to the movie, then please change your rating accordingly. </p>';  
} else if (ratingType === 'pleasant') {
  description = '<p>Pleasant is defined as your moment-to-moment experience of delight or gratification to the movie. Examples of items that are pleasurable are having a good meal, receiving a compliment from a friend, or acquiring money. If you experience a change in your level of delight to the movie, please change your rating accordingly.</p>';
} else if (ratingType === 'funny') {
  description = '<p>Funny is defined as your experience of how humorous the movie was to you. Examples of things that may be funny are a witty joke, ridiculous movement of the body, or a creative pun. If you experience a change in your level of amusement to the movie, please change your rating accordingly.</p>';
} else if (ratingType === 'unpleasant') {
  description = '<p>Unpleasant is defined as your moment-to-moment experience of aversion or distaste to the movie. Examples of things that may be unpleasant are a mean comment said to you, a bad-tasting meal, or a loud, screeching noise. If you experience a change in your level of aversion to the movie, please change your rating accordingly.</p>';
}
  const instructions = [
    'Click the button below if you would like to proceed to the video. If you have already watched the first video clip, use the button again to proceed to the next video clip. Your video will appear shortly. </p> <p> Remember the corresponding keys: </p> <p> <strong>Up arrow key:</strong> move rating bar up</p><p><strong>Down arrow key:</strong> move rating bar down</p> </p> <p> <strong> REMEMBER: </strong> </p> <p> You are rating the following emotion: <strong> ' + ratingType + ' </strong> </p> <strong> DESCRIPTION: </strong>'+ description + '<p> Please click the button below to proceed when you are ready. </p>'
  ];

  const dispatch = createEventDispatcher();
  let currentPage = 0;

  const backward = () => {
    dispatch('back');
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

   .next {
    background-color: lightblue;
	}

  .back {
    background-color: lightcoral;
  }
</style>
 
<div class="container">
  <h1 >Instructions</h1>
  <div class="text-box">
    <div class="content">
      {@html instructions[currentPage]}
    </div>
  </div>
  <br>
  <button class="back" on:click={backward}>Back</button>
  <button class="next" on:click={forward}>
    {#if currentPage === instructions.length - 1}
      Go To Task
    {:else}
      Next
    {/if}
  </button>
</div>



  