<script>
  import { score } from './store.js'

  export let question
  export let nextQuestion

  let isCorrect
  let isAnswered = false

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
    // isCorrect = correct
    isCorrect = true
    if (correct) score.update(currentValue => currentValue + 1)
  }
</script>

<h3>
  {@html question.question}
</h3>

{#if isAnswered}
  <h4 class:isCorrect>
    {#if isCorrect}Boa, vamos pra próxima 🎉{:else}Xiii, deu ruim 😬{/if}
  </h4>
{/if}

{#each allAnswers as answer}
  <button
    disabled={isAnswered}
    on:click={() => checkQuestion(answer.correct)}
  >
    {@html answer.answer}
  </button>
{/each}

{#if isAnswered}
  <div>
    <button on:click={nextQuestion}>Próxima pergunta</button>
  </div>
{/if}

<style>
  h4 {
    color: rgb(54, 33, 79);
  }
  h4.isCorrect {
    color: rgb(25, 31, 89);
  }
</style>