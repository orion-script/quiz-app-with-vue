<template>
  <div
    class="app-container p-6 min-h-screen bg-gray-100 flex flex-col items-center"
  >
    <h1 class="text-2xl font-bold mb-6">Mini Quiz App</h1>

    <!-- Multiple Choice Quiz -->
    <div class="quiz-section w-full max-w-md bg-white p-4 rounded-lg shadow-md">
      <h2 class="text-lg font-semibold">
        Question 1: What do plants need for photosynthesis?
      </h2>
      <div
        v-for="(option, index) in mcqOptions"
        :key="index"
        class="p-3 border rounded-lg mt-2 cursor-pointer"
        :class="{
          'bg-green-200': selectedAnswer === option && option === correctAnswer,
          'bg-red-200': selectedAnswer === option && option !== correctAnswer,
        }"
        @click="selectAnswer(option)"
      >
        {{ option }}
      </div>
      <p
        v-if="selectedAnswer"
        class="mt-3 text-sm"
        :class="{ 'text-green-700': isCorrect, 'text-red-700': !isCorrect }"
      >
        {{
          isCorrect
            ? "Right! Plants use sunlight to create food."
            : "Think again! Sunlight is essential for photosynthesis."
        }}
      </p>
    </div>

    <!-- Drag and Drop Section -->
    <div
      class="drag-drop-section w-full max-w-md bg-white p-4 rounded-lg shadow-md mt-6"
    >
      <h2 class="text-lg font-semibold">Match the Algebraic Terms</h2>
      <draggable
        v-model="terms"
        group="terms"
        itemKey="text"
        class="flex flex-wrap gap-2 mt-3"
      >
        <template #item="{ element }">
          <div class="p-3 bg-gray-300 rounded-lg cursor-grab">
            {{ element.text }}
          </div>
        </template>
      </draggable>
      <div class="mt-4">
        <button
          @click="checkDragDrop"
          class="bg-purple-500 text-white px-4 py-2 rounded-lg"
        >
          Submit
        </button>
      </div>
      <p
        v-if="dragDropFeedback"
        class="mt-3 text-sm"
        :class="{
          'text-green-700': dragDropCorrect,
          'text-red-700': !dragDropCorrect,
        }"
      >
        {{ dragDropFeedback }}
      </p>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import draggable from "vuedraggable";

export default {
  components: { draggable },
  setup() {
    // Multiple-choice question
    const mcqOptions = ref([
      "Oxygen & Sugar",
      "Sunlight, Water & Carbon Dioxide",
      "Proteins & Soil",
    ]);
    const correctAnswer = "Sunlight, Water & Carbon Dioxide";
    const selectedAnswer = ref(null);
    const isCorrect = ref(false);

    const selectAnswer = (option) => {
      selectedAnswer.value = option;
      isCorrect.value = option === correctAnswer;
    };

    // Drag and drop section
    const terms = ref([
      { text: "Variable", correct: true },
      { text: "Equation", correct: true },
      { text: "Constant", correct: false },
      { text: "Coefficient", correct: false },
    ]);
    const dragDropFeedback = ref("");
    const dragDropCorrect = ref(false);

    const checkDragDrop = () => {
      dragDropCorrect.value = terms.value.every((term) => term.correct);
      dragDropFeedback.value = dragDropCorrect.value
        ? "Right! You matched them correctly."
        : "Incorrect, try again!";
    };

    return {
      mcqOptions,
      selectedAnswer,
      isCorrect,
      correctAnswer,
      selectAnswer,
      terms,
      checkDragDrop,
      dragDropFeedback,
      dragDropCorrect,
    };
  },
};
</script>

<style>
.app-container {
  font-family: Arial, sans-serif;
}
</style>
