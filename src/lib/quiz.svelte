<script>
  import Modal from '$lib/modal.svelte'
  import Question from '$lib/question.svelte'
  import { fade } from 'svelte/transition'
  import { score } from './store.js'

  let quiz = getQuiz()
  let activeQuestion = 0
  let isModalOpen = false

  async function getQuiz() {
    const res = await fetch(
      'https://opentdb.com/api.php?amount=10&category=11&type=multiple'
    )
    const quiz = await res.json()
    return quiz
  }

  function nextQuestion() {
    activeQuestion = activeQuestion + 1
  }

  function resetQuiz() {
    isModalOpen = false
    score.set(0)
    quiz = getQuiz()
    activeQuestion = 0
  }

  // Reactive statement
  $: if ($score > 4) {
    isModalOpen = true
  }

  // Reactive declaration
  $: questionNumber = activeQuestion + 1
</script>

<div>
  <button on:click={resetQuiz}>Get new Questions</button>
  <h3>Score: {$score}</h3>
  <h4>Question #{questionNumber}</h4>

  {#await quiz}
    Loading...
  {:then data}
    {#each data.results as question, index}
      {#if index === activeQuestion}
        <div transition:fade class="fade-wrapper">
          <Question {question} {nextQuestion} />
        </div>
      {/if}
    {/each}
  {/await}
</div>

{#if isModalOpen}
  <Modal on:close={resetQuiz}>
    <h2>You Won! 🎉</h2>
    <p>Congrats!</p>
    <button on:click={resetQuiz}>Start Over</button>
  </Modal>
{/if}

<style>
  /* text-align: center; */
  .fade-wrapper {
    position: relative;
    transition: all 300ms;
  }
</style>
