<!-- this page handles the intro -->

<script>
    import {totalHITTime, estHITTime, HITPay, studyTasks} from '../utils.js'
</script>

<style>
    .container {
        text-align: center;
        align-items: center;
        width: 50%;
        margin: 0 auto !important; 
    }
    .text-box {
        text-align: left;
        padding: 2%;
        background-color: rgba(255, 255, 255, 0.4);
        border: 2px solid grey;
        border-radius: 2px;
        box-shadow: 2px 2px 8px rgba(0, 0, 0, 0.1);   
    }
</style>


<div class="container">
    <h1>Study Preview</h1>
    <div class="text-box">
        <p>This study involves {studyTasks}.</p>
        <p>You are given {totalHITTime} minutes to complete this study, but it should only take about {estHITTime}.</p>
        <p>This study pays ${HITPay}.</p>
        <p>Please accept this study if you are interested!</p>
    </div>
</div>



