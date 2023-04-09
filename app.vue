<template>
  <div class="container mx-auto mt-10">
    <h1 class="text-4xl font-bold mb-4">Speech to Text</h1>
    <textarea
      v-model="transcript"
      rows="5"
      class="w-full p-4 bg-gray-200 rounded-md focus:outline-none focus:bg-white"
    ></textarea>
    <div class="mt-4 ">
      <button
        @click="startRecording"
        :disabled="isRecording"
        class="px-4 py-2 bg-green-500 text-white rounded-md hover:bg-green-600"
      >
        Start Recording
      </button>
      <button
        @click="stopRecording"
        :disabled="!isRecording"
        class="px-4 py-2 bg-red-500 text-white rounded-md hover:bg-red-600"
      >
        Stop Recording
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isRecording: false,
      transcript: ""
    };
  },
  methods: {
    startRecording() {
      this.isRecording = true;
      // Mulai merekam suara
      const recognition = new window.webkitSpeechRecognition();
      recognition.start();
      recognition.onresult = event => {
        this.transcript = event.results[0][0].transcript;
      };
      recognition.onend = () => {
        this.isRecording = false;
      };
    },
    stopRecording() {
      this.isRecording = false;
      // Stop merekam suara
      window.webkitSpeechRecognition.abort();
    }
  }
};
</script>


