<script>
  import { run } from 'svelte/legacy';

  import Modal from '$lib/modal.svelte'
  import Question from '$lib/question.svelte'
  import { fade, fly } from 'svelte/transition'
  import { score } from './store.js'

  let quiz = $state(getQuiz())
  let activeQuestion = $state(0)
  let isModalOpen = $state(false)

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
  run(() => {
    if ($score > 4) {
      isModalOpen = true
    }
  });

  // Reactive declaration
  let questionNumber = $derived(activeQuestion + 1)
</script>

<div>
  <button onclick={resetQuiz}>Get new Questions</button>
  <h3>Score: {$score}</h3>
  <h4>Question #{questionNumber}</h4>

  {#await quiz}
    Loading...
  {:then data}
    {#each data.results as question, index}
      {#if index === activeQuestion}
        <div
          out:fade
          in:fly={{ y: 200, duration: 1000 }}
          class="fade-wrapper"
        >
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
    <button onclick={resetQuiz}>Start Over</button>
  </Modal>
{/if}

<style>
  /* text-align: center; */
  .fade-wrapper {
    position: relative;
    transition: all 300ms;
  }
</style>
