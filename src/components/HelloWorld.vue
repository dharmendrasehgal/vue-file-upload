<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <input style="display: none" ref="fileInput" type="file" @change="onFileSelected" />
    <button @click="$refs.fileInput.click()">Pick File</button>
    <button @click="onUpload">Upload File</button>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
        selectedFile: null
    }
  },
  methods: {
    onFileSelected(event) {
        /* eslint-disable no-console */
        console.log(event);
        this.selectedFile = event.target.files[0];
    },
    onUpload() {
        const fd = new FormData();
        fd.append('image', this.selectedFile, this.selectedFile.name);
        axios.post('https://api.imgur.com/3/image', fd, {
            onUploadProgress: uploadEvent => {
                console.log('Upload Progress: ', Math.round(uploadEvent.loaded / uploadEvent.total * 100) + '%');
            }
        })
            .then( res => {
                /* eslint-disable no-console */
                console.log(res);
            })
            .catch( err => {
                /* eslint-disable no-console */
                console.log("Uploading Error: ",err);
            })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
