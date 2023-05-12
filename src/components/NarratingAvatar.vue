<template>
  <div class="w-100 h-100 m-5">
    <!-- inputing text and submitting -->
    <textarea v-model="textInput" rows="20" cols="100"></textarea>
    <button @click="speak" class="btn btn-primary">Read Text</button>
    <button @click="stop" class="btn btn-danger">Stop Talking</button>

    <!-- Displaying text as it is being read -->
    <div>
      <span
        v-for="(word, index) in words"
        :key="index"
        :class="{ highlighted: index === currentWordIndex }"
      >
        {{ word }} <span> </span>
      </span>
    </div>

    <!-- showing letters as they are being spoken -->
    <div>
      <span
        v-for="(char, index) in characters"
        :key="index"
        :class="{ 'highlighted-pink': index === currentLetterIndex }"
      >
        {{ char }}
      </span>
    </div>
    <!-- Phonemes -->
    <img :src="currentPhoneme" />
  </div>
</template>

<script>
import imageAEI from "../assets/AEI.png";
import imageBMP from "../assets/BMP.png";
import imageN from "../assets/Nn.png";
import imageCDGKNRSTXYZ from "../assets/CDGKNRSTXYZ.png";
import imageCHJSONSH from "../assets/CH,J,SH.png";
import imageFV from "../assets/FV.png";
import imageL from "../assets/L.png";
import imageO from "../assets/O.png";
import imageQW from "../assets/QW.png";
import imageTH from "../assets/TH.png";
import imageU from "../assets/U.png";
import imageE from "../assets/E.png";

export default {
  data() {
    return {
      textInput:
        "“Poor Donnie; he was man caught in the wrong place at the wrong time. He died too young. We’ll miss him”… There. That could have been Donnie’s eulogy. Instead, John Goodman’s Walter takes what should’ve been a simple, thoughtful speech to places that only his dark mind could go. Contrasting Jeff Bridges' passive, mellowed-out Dude, Walter’s a man who never got over Vietnam. Quick to anger, he begins Donnie’s eulogy with an attempt to provide a beautiful and poised tribute – but it only comes off as awkward. He then quickly moves past his departed friend’s memorial to lambaste god for the lives lost in Vietnam. It takes a lot to get The Dude mad, but this hilariously deadpan scene gets the job done.",
      words: [],
      characters: [],
      currentWordIndex: -1,
      currentLetterIndex: 0,
      speech: new SpeechSynthesisUtterance(),
      currentPhoneme: "",
      letterMapping: {
        a: imageAEI,
        b: imageBMP,
        c: imageCDGKNRSTXYZ,
        ch: imageCHJSONSH,
        d: imageFV,
        e: imageL,
        f: imageO,
        g: imageQW,
        h: imageTH,
        i: imageU,
        j: imageN,
        k: imageE,
        l: imageAEI,
        m: imageBMP,
        n: imageCDGKNRSTXYZ,
        o: imageCHJSONSH,
        p: imageFV,
        q: imageL,
        r: imageO,
        s: imageQW,
        t: imageTH,
        u: imageU,
        v: imageN,
        w: imageE,
        x: imageAEI,
        y: imageBMP,
        z: imageCDGKNRSTXYZ,
      },
    };
  },
  computed: {},
  methods: {
    stop() {
      speechSynthesis.cancel();
    },
    speak() {
      this.startGettingLetters();
      this.words = this.textInput.trim().split(/\s+/);

      this.currentWordIndex = 0;

      this.speech.text = this.textInput;
      this.speech.rate = 0.7;

      this.speech.onboundary = this.highlightWord;
      speechSynthesis.speak(this.speech);
    },
    highlightWord(event) {
      if (event.name === "word") {
        this.currentWordIndex = this.textInput
          .substring(-1, event.charIndex)
          .trim()
          .split(/\s+/).length;
      }
    },
    startGettingLetters() {
      this.characters = this.textInput.trim().split("");
      const delay = 95;
      this.characters.forEach((char, index) => {
        setTimeout(() => {
          this.currentLetterIndex = index;
          this.currentPhoneme = this.letterMapping[char];
        }, index * delay);
      });
    },
  },
};
</script>

<style scoped>
.highlighted {
  background-color: yellow;
}
.highlighted-pink {
  background-color: pink;
}
</style>
