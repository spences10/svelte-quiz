<script>
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
</script>

<button on:click={resetQuiz}>Get new Questions</button>
<h3>Score: {score}</h3>
<h4>Question #{activeQuestion + 1}</h4>

<div>

  {#await quiz}
    Loading...
  {:then data}
    {#each data.results as question, index}
      {#if index === activeQuestion}
        <Qusetion {question} {nextQuestion} {addToScore} />
      {/if}
    {/each}
  {/await}

</div>
