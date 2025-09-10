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
        let answer;

        export let ratingType;
	    export let movies;
	    export let options;
	    export let links;
	    export let index; 

        let emailAddress = "mailto:" + email;
        let currID = params.assignmentId;
        let postURL = 'https://www.prolific.com/'
        let moviesRemaining = [];
        let numVideos = 9;
        
        export const ses_num = answer;

        let currVid;
	    let currVidSrc;
	    let ratingDocPathway;
        let botCheck = 2;
        let prolific_id;

        export let subPath;

        if (options > 0) {
		// choose random movie and rating type
		currVid = movies[index];

		let vidPlusRating = `${currVid}-${ratingType}`;

		ratingDocPathway = `${ratingsPath}/${params.workerId}/${vidPlusRating}`;

		// grab URL for video sourcing
		currVidSrc = links[index];
	
	}
    
    console.log(ratingDocPathway)


     
        const newPage = async () =>{   
           
                dispatch("finished")
                await db.doc(subPath).update({
                   Prolific_ID: prolific_id,

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
                <h2> Prolific Identification </h2>
                <p> Thank you for participating in this study. Please provide your Prolific ID below to help us ensure you are compensated for your time. </p>              
                <em> Please input your ID correctly into the input box below. </em>
                <br> <br>

                <input type="hidden" name="assignmentId" id="assignmentId" value={currID}>
                <input type="hidden" name="hidden_val_DONT_REMOVE" value="1">

                <label class="label"><u>Prolific ID</u>
                    <div class="options">

                    <input
                        class="input lang-input"
                        type="text"
                        bind:value={prolific_id}
                        placeholder="Prolific ID" />
                    </div>
                    <br>
            
               
                        
                <div class="field-label">
                    <!-- Left empty for spacing -->
                </div> 
                <br>
                <button class="button is-success is-large" on:click={newPage}>NEXT PAGE</button>         
            </form>
        </div> 
    </div>
