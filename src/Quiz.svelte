<script>
  import { fade } from "svelte/transition";
  import Qusetion from "./Question.svelte";

  let quiz = getQuiz();
  let activeQuestion = 0;
  let score = 0;

  async function getQuiz() {
    const res = await fetch(
      "https://opentdb.com/api.php?amount=10&category=11&type=multiple"
    );
    const quiz = await res.json();
    return quiz;
  }

  function nextQuestion() {
    activeQuestion = activeQuestion + 1;
  }

  function resetQuiz() {
    score = 0;
    quiz = getQuiz();
    activeQuestion = 0;
  }

  function addToScore() {
    score = score + 1;
  }

  // Reactive statement
  $: if (score > 7) {
    alert(`You won! yay 🎉`);
    resetQuiz();
  }

  // Reactive declaration
  $: questionNumber = activeQuestion + 1;
</script>

<style>
  /* text-align: center; */
  .fade-wrapper {
    position: relative;
    transition: all 300ms;
  }
</style>

<div>
  <button on:click={resetQuiz}>Get new Questions</button>
  <h3>Score: {score}</h3>
  <h4>Question #{questionNumber}</h4>

  {#await quiz}
    Loading...
  {:then data}
    {#each data.results as question, index}
      {#if index === activeQuestion}
        <div transition:fade class="fade-wrapper">
          <Qusetion {question} {nextQuestion} {addToScore} />
        </div>
      {/if}
    {/each}
  {/await}

</div>
