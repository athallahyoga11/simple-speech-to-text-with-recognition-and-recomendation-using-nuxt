<template>
  <div class="container mx-auto mt-10">
    <h1 class="text-4xl font-bold mb-4">Speech to Text</h1>
    <select v-model="selectedLang" class="border p-2 mb-4">
      <option value="en-US">English (US)</option>
      <option value="id-ID">Indonesia</option>
    </select>
    <textarea
      v-model="transcript"
      rows="5"
      class="w-full p-4 rounded-3xl bg-gray-400 rounded-md bg-clip-padding backdrop-filter backdrop-blur-sm bg-opacity-10  focus:outline-none "
    >
    </textarea>
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
      <button @click="removeResult" :disabled="!result" class="bg-gray-500 text-white py-2 px-4 rounded-md cursor-pointer mt-4">Remove</button>
    </div>
    <div v-if="transcript">
      <p class="mb-2"><strong>Transkrip:</strong> {{ transcript }}</p>
      <p class="mb-2"><strong>Label:</strong> {{ label }}</p>
      <p class="mb-2"><strong>Rekomendasi:</strong></p>
      <ul>
        <li v-for="recommendation in recommendations" :key="recommendation">{{ recommendation }}</li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      text : '',
      recognition : null,
      result : '',
      selectedLang : 'en-US',
      isRecording: false,
      transcript: ""
    };
  },
  methods: {
    onInputChange() {
      this.result = ''
    },
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
    },
    onRecognitionResult(event) {
      let result = ''
      for (let i = event.resultIndex; i < event.results.length; i++) {
        result += event.results[i][0].transcript
      }
      this.result = result
    },
    removeResult() {
      this.result = ''
    }
  }
};
</script>


