<script setup>
</script>

<template>
  <div class="base64-encoder">
    <h1>Encode to Base64</h1>
    <p>Just enter your data and click the encode button.</p>
    <textarea v-model="inputData" placeholder="Type (or paste) here..."></textarea>
    
    <!-- <div>
      <input type="radio" id="binary" value="binary" v-model="dataType">
      <label for="binary">For encoded binaries such as images, documents, etc., use the file upload form located further down on this page.</label>
    </div> -->
    
    <div>
      <select id="charset" v-model="charset">
        <!-- <option disabled value="">Destination character set.</option> -->
        <option value="UTF-8">UTF-8</option>
        <option value="ASCII">ASCII</option>
        <option value="ISO-8859-1">ISO-8859-1</option>
        <option value="ISO-8859-2">ISO-8859-2</option>
        <option value="ISO-8859-6">ISO-8859-6</option>
        <option value="ISO-8859-15">ISO-8859-15</option>
        <option value="Windows-1252">Windows-1252</option>
      </select>
      <label for="charset">Destination character set. </label>
    </div>
    <div>
      <input type="checkbox" id="encodeLines" v-model="encodeEachLine">
      <label for="encodeLines">Encode each line separately (useful for handling multiple entries).</label>
    </div>

    <button @click="encode">Encode</button>
    <textarea readonly :value="encodedData" placeholder="Result goes here..."></textarea>

    <button @click="copyToClipboard">Copy to clipboard</button>
    <div class="viewmore"><a target="_blank" href="https://b64encode.org">View more</a></div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      inputData: '',
      encodedData: '',
      dataType: 'text', // or 'binary' if you plan to handle files
      charset: 'UTF-8',
      encodeEachLine: false
    };
  },
  methods: {
    encode() {
      let dataToEncode = this.inputData;
      if (this.encodeEachLine) {
        dataToEncode = dataToEncode.split('\n').map(line => btoa(unescape(encodeURIComponent(line)))).join('\n');
      } else {
        dataToEncode = btoa(unescape(encodeURIComponent(dataToEncode)));
      }
      this.encodedData = dataToEncode;
    },
    copyToClipboard() {
      navigator.clipboard.writeText(this.encodedData)
        .then(() => alert('Copied to clipboard!'))
        .catch(err => alert('Failed to copy: ' + err));
    }
  }
};
</script>
<style scoped>
.base64-encoder {
  max-width: 800px;
  margin: auto;
  /* padding: 20px; */
  /* border: 1px solid #ccc; */
  /* border-radius: 8px; */
}

.base64-encoder textarea {
  width: 100%;
  min-height: 100px;
  margin-bottom: 10px;
}

.base64-encoder button {
  margin: 10px 5px;
  padding: 5px 20px;
}

.base64-encoder select, .base64-encoder input[type="checkbox"], .base64-encoder input[type="radio"] {
  margin: 10px 0;
}
.viewmore{
  text-align: right;
}
</style>
