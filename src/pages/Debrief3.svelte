<!-- this page is the debrief that collects post survey questions;
    there is a single button that saves responses to firebase and submits HIT  -->

    <script>
        import { db, params, serverTime, experiment, userGroup } from '../utils'
        import { createEventDispatcher } from 'svelte';


        const ratingsPath = `${experiment}/ratings`;
	    const ratingsDoc = db.doc(ratingsPath);
	    const subjectGroupPath = `${experiment}/subjects/${userGroup}`;
	    const subjectGroupCollection = db.collection(subjectGroupPath);
	    const stimuliPath = `${experiment}/stimuli`;
	    const stimuliDoc = db.doc(stimuliPath);

        const dispatch = createEventDispatcher();
        
        // populating necessary variables
        
        export let email;
        export let videoIndex;
  
        export let ratingType;
	    export let movies;
	    export let options;
	    export let links;
	    export let index; 
     

        let emailAddress = "mailto:" + email;
        let currID = params.assignmentId;
        let postURL = 'https://www.prolific.com/'
        let moviesRemaining = [];
        let numVideos = 3;

        let currVid;
	    let currVidSrc;
	    let ratingDocPathway;
        let botCheck = 2;
       let answer = '';

        if (options > 0) {
		// choose random movie and rating type
		currVid = movies[index];

		let vidPlusRating = `${currVid}-${ratingType}`;

		ratingDocPathway = `${ratingsPath}/${params.workerId}/${vidPlusRating}`;

		// grab URL for video sourcing
		currVidSrc = links[index];
	
        }
        let Main_question = ['1. Which of the following topics was NOT mentioned or discussed in the previous video?', '1.   Who is the biggest enemy of Kung Fury?'];

let Answer_a = ['A) Truman Doctrine', 'A) The police department'];
let Answer_b = ['B) Spanish-American War', 'B) Barbariana'];
let Answer_c = ['C) Vietnam and Korea', 'C) An electronic computer']
let Answer_d = ['D) Berlin Wall','D) Thor'];
let Answer_e =['E) Ronald Raegan', 'E) Adolf Hitler'];
console.log(ratingDocPathway)



     
        const newPage = async () =>{   
            if (videoIndex % botCheck == 0 && videoIndex != numVideos){
            
                dispatch("botcheck")
                await db.doc(ratingDocPathway).update({
                Comphrehension: answer
                });    
            }   else

                dispatch("finished")
                await db.doc(ratingDocPathway).update({
                   Comphrehension: answer
                
                });    
        
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
    </style>
    
    <div class="container">
        <div class="form-box">
            <form name="mturk" action={postURL} method='POST'>
                <h2> Please answer the following question, then press “NEXT PAGE” to continue to the next video.  </h2>
           

                <input type="hidden" name="assignmentId" id="assignmentId" value={currID}>
                <input type="hidden" name="hidden_val_DONT_REMOVE" value="1">


                                        <label class="label"
            ><u>       {Main_question[videoIndex-1]}  </u>
            <div class="options">
                <label class="radio">
                    <input type="radio" bind:group={answer} value={"A"} />
                    {Answer_a[videoIndex-1]}
                </label>
                <label class="radio">
                    <input type="radio" bind:group={answer} value={"B"} />
                    {Answer_b[videoIndex-1]}                </label>
                <label class="radio">
                    <input type="radio" bind:group={answer} value={"C"} />
                    {Answer_c[videoIndex-1]}  
                </label>
                <label class="radio">
                    <input type="radio" bind:group={answer} value={"D"} />
                    {Answer_d[videoIndex-1]}  
                </label>
                <label class="radio">
                    <input type="radio" bind:group={answer} value={"E"} />
                    {Answer_e[videoIndex-1]}  
                </label>
                <br />
            </div>
        </label>

                 
                
                        
                <div class="field-label">
                    <!-- Left empty for spacing -->
                </div> 
                <br>
                <button class="button is-success is-large" on:click={newPage}>NEXT PAGE</button>         
            </form>
        </div> 
    </div>
