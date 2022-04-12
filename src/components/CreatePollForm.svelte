<script>
  import PollStore from "../stores/PollStore";
  import Button from "../shared/Button.svelte";
  import { createEventDispatcher } from "svelte";

  let dispatch = createEventDispatcher();
  let fields = {
    question: "",
    answerA: "",
    answerB: "",
  };
  let errors = {
    question: "",
    answerA: "",
    answerB: "",
  };
  let errorClass = {
    question: false,
    answerA: false,
    answerB: false,
  };
  let valid = false;

  const submitHandler = () => {
    valid = true;

    if (fields.question.trim().length < 5) {
      valid = false;
      errors.question = "Question must be at least 5 characters long";
      errorClass.question = true;
    } else {
      errors.question = "";
      errorClass.question = false;
    }

    if (fields.question.trim().length < 1) {
      valid = false;
      errors.answerA = "Answer A must be at least 5 characters long";
      errorClass.answerA = true;
    } else {
      errors.answerA = "";
      errorClass.answerA = false;
    }

    if (fields.question.trim().length < 1) {
      console.log("answerb validator");
      valid = false;
      errors.answerB = "Answer B must be at least 5 characters long";
      errorClass.answerB = true;
    } else {
      errors.answerB = "";
      errorClass.answerB = false;
    }

    if (valid) {
      let poll = { ...fields, votesA: 0, votesB: 0, id: Math.random() };
      PollStore.update((currentPolls) => {
        return [poll, ...currentPolls];
      });
      dispatch("add");
      console.log("valid");
    }
  };
</script>

<form on:submit|preventDefault={submitHandler}>
  <div class="form-field">
    <label for="question"> Poll Question</label>
    <input
      type="text"
      id="question"
      bind:value={fields.question}
      class:error={errorClass.question}
    />
    <div class="error">{errors.question}</div>
  </div>
  <div class="form-field">
    <label for="answer-a"> Answer A:</label>
    <input
      type="text"
      id="answer-a"
      bind:value={fields.answerA}
      class:error={errorClass.answerA}
    />
    <div class="error">{errors.answerA}</div>
  </div>
  <div class="form-field">
    <label for="answer-b"> Answer B:</label>
    <input
      type="text"
      id="answer-b"
      bind:value={fields.answerB}
      class:error={errorClass.answerB}
    />
    <div class="error">{errors.answerB}</div>
  </div>
  <Button type="secondary" flat={true}>Add Poll</Button>
</form>

<style>
  form {
    width: 400px;
    margin: 0 auto;
    text-align: center;
  }
  .form-field {
    margin: 18px auto;
  }
  input {
    width: 100%;
    border-radius: 6px;
  }
  label {
    margin: 10px auto;
    text-align: left;
  }
  div.error {
    font-weight: bold;
    font-size: 12px;
    color: #d91b42;
  }
</style>
