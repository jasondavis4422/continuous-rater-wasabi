<script>
  import { db, params, experiment, userGroup } from '../utils';
  import { createEventDispatcher } from 'svelte';

  const ratingsPath = `${experiment}/ratings`;
  const dispatch = createEventDispatcher();

  export let email;
  export let videoIndex;
  export let ratingType;
  export let movies;
  export let links;
  export let index;
  export let options;

  let emailAddress = "mailto:" + email;
  let currID = params.assignmentId;
  let postURL = 'https://www.prolific.com/';
  let numVideos = 3;
  let botCheck = 3;

  let currVid;
  let currVidSrc;
  let ratingDocPathway;

if (options > 0) {
    currVid = movies[index];
    let vidPlusRating = `${currVid}-${ratingType}`;
    ratingDocPathway = `${ratingsPath}/${params.workerId}/${vidPlusRating}`;
    currVidSrc = links[index];
} else {
    currVid = movies[videoIndex - 1];
    let vidPlusRating = `${currVid}-${ratingType}`;
    ratingDocPathway = `${ratingsPath}/${params.workerId}/${vidPlusRating}`;
    currVidSrc = links[videoIndex - 1];
}

  // Provide five question strings for the current video (index into arrays by videoIndex-1)
  let Main_questions = [
    // video 1: five questions
    [
      '1. What is the lowest anchor point on the rating scale?',
      '2. What is the highest anchor point on the rating scale?',
      '3. How frequently should you rate your emotional experience?',
      '4. How many rating points did you move the scale between in the training video?',
      '5. What are you rating while watching the movie?'
    ],
    // video 2: five questions (placeholder text)
    [
      '1. Who is the biggest enemy of Kung Fury?',
      '2. What event triggers Kung Fury to seek revenge?',
      '3. Who is Hackerman and what role does he play early on?',
      '4. How does Kung Fury get his nickname?',
      '5. What is the significance of the time machine in the first half of the movie?'
    ],
    // video 3: if you have a third video, add five questions; otherwise placeholders
    [
      '1. What major confrontation occurs near the end of the film?',
      '2. Which allies join Kung Fury in the final battle?',
      '3. What does Kung Fury use to stay in contact with the two women?',
      '4. What role does Hackerman play in the climax of the film?',
      '5. Who saves Adolf Hitler from dying?'
    ]
  ];

  // Option labels for each video: array of arrays for A-E per question block
  // We'll reuse the same options across the five questions for simplicity;
  // you can create a structure per question if needed.
  let Answer_a = [
    ['A) 20','A) 40','A) not very frequently','A) 20','A) Your overall emotional state'],
    ['A) The police department','A) His dojo being destroyed','A) A corrupt cop who betrays Kung Fury','A) He wins a city-wide tournament','A) It is merely a background prop with no real use'],
    ['A) Kung Fury fights his former mentor','A) Only Hackerman arrives to help','A) A transportable, cellular phone','A) He betrays Kung Fury and joins the villains','A) intelligent computer hacker']
  ];

  let Answer_b = [
    ['B) 40','B) 60','B) never at all','B) 10 ','B) Your moment by moment subjective emotional experience'],
    ['B) Barbariana','B) His mentor abandoning him','B) A rival martial artist challenging Kung Fury','B) He kills a Viking with his bare hands and is praised by colleagues','B) It is used to transport Kung Fury to a secret island'],
    ['B) Kung Fury duels Hackerman in cyberspace','B) Several allies including Vikings, a T-Rex, and other time-displaced fighters','B) An online video platform','B) He is captured and never seen again','B) A golden eagle']
  ];

  let Answer_c = [
    ['C) 100','C) 0','C) moment-by moment','C) 5','C) The emotional state of your friend'],
    ['C) An electronic computer','C) A bank robbery gone wrong','C) A hacker who uses computers to help send Kung Fury back in time','C) He defeats the entire mafia','C) It is used to travel back in time to fight Adolf Hitler'],
    ['C) Kung Fury battles Adolf Hitler (Kung Führer)','C) A squad of modern SWAT officers','C) He does not stay in contact with the two women','C) He assists with time/tech manipulation to support the victory','C) A giant dinosaur']
  ];

  let Answer_d = [
    ['D) 0','D) 30','D) somewhat frequently','D) 15','D) The sociality of the main character'],
    ['D) Thor','D) A Nazi Colonel killing his police partner','D) The abusive trainer of Kung Fury','D) He rescues the president','D) It is stolen by the Viking and never recovered'],
    ['D) Kung Fury faces a giant dragon','D) Just his original police precinct','D) A landline','D) He reveals he is actually the main villain','D) Kung Fury']
  ];

  let Answer_e = [
    ['E) 60','E) 100','E) Ratings were not necessary','E) 100','E) Whether the side character is tall or short'],
    ['E) Adolf Hitler','E) A rival fighter defeating him','E) A government agent tracking Nazis','E) He masters a secret martial art','E) It malfunctions and leaves Kung Fury trapped in the present'],
    ['E) Kung Fury is arrested by the police','E) No one — he fights alone','E) A physical mail service','E) He fights Kung Fury hand-to-hand','E) A giant robot']
  ];

  // Answers array for five questions; initialize to empty strings
  let answers = ['', '', '', '', ''];

  // Save function: writes five answers to the Firestore document
  async function saveAndContinue(event) {
    event.preventDefault();

    if (!ratingDocPathway) {
      console.error('ratingDocPathway not set');
      return;
    }

    // Build object to write: q1..q5 (or change keys as needed)
    const compObj = {
      q1: answers[0],
      q2: answers[1],
      q3: answers[2],
      q4: answers[3],
      q5: answers[4],
      // optionally include timestamp
      updatedAt: new Date()
    };

    try {
      // Use doc reference and update (or set if you want to merge)
      const docRef = db.doc(ratingDocPathway);
      await docRef.update({
        Comprehension: compObj
      });

      // dispatch event depending on botcheck logic
      if (videoIndex < numVideos) {
        dispatch('finished');
      } else {
        dispatch('debrief');
      }

      // If you want to continue to the POST URL (e.g., external submit),
      // you can redirect here after saving.
      // window.location.href = postURL;

    } catch (err) {
      console.error('Error saving comprehension answers:', err);
      // If document doesn't exist, you might prefer to set() with merge:
      // await docRef.set({ Comprehension: compObj }, { merge: true });
    }
  }
</script>

<style>
  .container {
    margin: 0 auto !important;
    max-width: 800px;
    text-align: center;
  }
  .form-box {
    padding: 2%;
    background-color: rgba(255, 255, 255, 0.6);
    border-left: 2px solid #aaa;
    border-radius: 2px;
    box-shadow: 2px 2px 8px rgba(0, 0, 0, 0.2);
    text-align: left;
  }
  .label {
    font-weight: bold;
  }
  .button {
    background-color: lightblue;
  }
  .question-block {
    margin-bottom: 1rem;
    padding: 0.5rem;
    border-bottom: 1px solid #eee;
  }
</style>

<div class="container">
  <div class="form-box">
    <form on:submit|preventDefault={saveAndContinue}>
      <h2>Please answer the following questions, then press “NEXT PAGE” to continue to the next video.</h2>

      <input type="hidden" name="assignmentId" id="assignmentId" value={currID} />
      <input type="hidden" name="hidden_val_DONT_REMOVE" value="1" />

      {#if Main_questions[videoIndex - 1]}
        {#each Main_questions[videoIndex - 1] as question, i}
          <div class="question-block">
            <label class="label"><u>{question}</u></label>
            <div class="options">
              <label class="radio">
                <input type="radio" bind:group={answers[i]} value={"A"} />
                {Answer_a[videoIndex - 1][i]}
              </label>
              <label class="radio">
                <input type="radio" bind:group={answers[i]} value={"B"} />
                {Answer_b[videoIndex - 1][i]}
              </label>
              <label class="radio">
                <input type="radio" bind:group={answers[i]} value={"C"} />
                {Answer_c[videoIndex - 1][i]}
              </label>
              <label class="radio">
                <input type="radio" bind:group={answers[i]} value={"D"} />
                {Answer_d[videoIndex - 1][i]}
              </label>
              <label class="radio">
                <input type="radio" bind:group={answers[i]} value={"E"} />
                {Answer_e[videoIndex - 1][i]}
              </label>
            </div>
          </div>
        {/each}
      {:else}
        <p>No questions configured for this video index.</p>
      {/if}

      <div class="field-label"></div>
      <br />
      <button class="button is-success is-large" type="submit">NEXT PAGE</button>
    </form>
  </div>
</div>