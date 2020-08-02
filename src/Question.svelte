<script>
  export let question;
  export let nextQuestion;
  export let addToScore;

  let isCorrect;
  let isAnswered = false;

  let answers = question.incorrect_answers.map((answer) => {
    return {
      answer,
      correct: false,
    };
  });
  let allAnswers = [
    ...answers,
    {
      answer: question.correct_answer,
      correct: true,
    },
  ];
  shuffle(allAnswers);
  function shuffle(array) {
    array.sort(() => Math.random() - 0.5);
  }

  function checkQuestion(correct) {
    isAnswered = true;
    isCorrect = correct;
    if (correct) addToScore();
  }
</script>

<h3>
  {@html question.question}
</h3>

<h4>
  {#if isAnswered}
    {#if isCorrect}Correct 🎉{:else}Wrong 😬{/if}
  {/if}
</h4>

{#each allAnswers as answer}
  <button disabled={isAnswered} on:click={() => checkQuestion(answer.correct)}>
    {@html answer.answer}
  </button>
{/each}

{#if isAnswered}
  <div>
    <button on:click={nextQuestion}>Next Question</button>
  </div>
{/if}
