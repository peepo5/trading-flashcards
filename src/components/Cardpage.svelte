<script>
    import { loop_guard } from "svelte/internal";
    import App from "../App.svelte";

    export let card_data;
    let cards = card_data['cards'];
    console.log(`Amount of cards -> ${cards.length}`);

    let card = cards[0];
    let card_index = 0;
    let right_answer = card["answers"][0];

    let beginned = false;
    let finished_card = false;

    const random_card_gen = () => {
        console.log(`Cards left: ${cards.length}`);

        finished_card = false;
        wipe_colors();

        card_index = Math.floor(Math.random()*cards.length);

        card = cards[card_index];
        console.log(card["answers"]);
        right_answer = card["answers"][0];
        card["answers"].sort(function() { return 0.5 - Math.random() });
        console.log(card["answers"]);
    }

    const wipe_colors = () => {
        for(let x=0; x <= 5; x++) {
            let button = document.getElementById(`e${x}`);
            if(button != undefined) {
                button.style.background = "#353535";
            }
        }
    }

    const check_answers = (answer, i) => {
        if(finished_card == true){
            throw "If you see this VI VON"
        }
        let clicked_card = document.getElementById(`e${i}`);
        if(answer[1] == true) {
            console.log("yes");
            clicked_card.style.background = "green";
            finished_card = true;
        } else {
            console.log("no");
            clicked_card.style.background = "black"; //#d32f2f
        }
    }

</script>

<main>
    <h1>{card_data["title"]} Flashcards</h1>
    <p class="subtext">{card_data["description"]} Cards left: {cards.length}</p>
    <button id="begin-button" on:click={() => {random_card_gen(); beginned = true}} hidden={beginned}>Begin</button>

    <hr hidden={!beginned}>

    <div class="card-section no-select" hidden={!beginned}>
        {#if card["img"] != undefined}
        <img src="../card_media/{card['img']}" alt="card_media" style="max-width:600px;">
        {/if}
        <p style="padding: 8px;"><b>Question:</b> {card["question"]}</p>
        {#each card["answers"] as answer, i}
        <div class="answer-cont" on:click={() => check_answers(answer, i)} on:keypress={() => check_answers(answer, i)}>
            <p class="answer" id="e{i}">{answer[0]}</p>
        </div>
        {/each}
    </div>

    <div hidden={!finished_card}>
    <div class="flex-container">
        <img src="../information-circle-outline.svg" class="invert info" height=40px alt="info">
        <p style="max-width: 350px;">{card["explanation"]}</p>
    </div>
    <button on:click={() => {cards.splice(card_index, 1); cards=cards; random_card_gen()}}>Next Card</button>
    </div>
</main>