<script>
  import { score } from './store.js'

  let { question, nextQuestion } = $props();

  let isCorrect = $state()
  let isAnswered = $state(false)

  let answers = question.incorrect_answers.map(answer => {
    return {
      answer,
      correct: false,
    }
  })
  let allAnswers = [
    ...answers,
    {
      answer: question.correct_answer,
      correct: true,
    },
  ]
  shuffle(allAnswers)
  function shuffle(array) {
    array.sort(() => Math.random() - 0.5)
  }

  function checkQuestion(correct) {
    isAnswered = true
    isCorrect = correct
    if (correct) score.update(currentValue => currentValue + 1)
  }
</script>

<h3>
  {@html question.question}
</h3>

{#if isAnswered}
  <h4 class:isCorrect>
    {#if isCorrect}Correct 🎉{:else}Wrong 😬{/if}
  </h4>
{/if}

{#each allAnswers as answer}
  <button
    disabled={isAnswered}
    onclick={() => checkQuestion(answer.correct)}
  >
    {@html answer.answer}
  </button>
{/each}

{#if isAnswered}
  <div>
    <button onclick={nextQuestion}>Next Question</button>
  </div>
{/if}

<style>
  h4 {
    color: red;
  }
  h4.isCorrect {
    color: green;
  }
</style>
