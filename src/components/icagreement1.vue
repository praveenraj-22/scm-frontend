<template>
  <div class="container">
    <div class="large-12 medium-12 small-12 cell">
      <!-- <label>File
        <input type="file"  ref="agrrement"  accept="image/x-png, image/gif, image/jpeg,application/pdf"  v-on:change="handleFileUpload()"/>
      </label>
      <button v-on:click="submitFile()">Submit</button> -->

      <v-text-field type='file' clearable @click="testing" v-model="file" label="Bank Name" required></v-text-field>

    </div>
  </div>
</template>

<script>
  export default {
    /*
      Defines the data used by the component
    */
    data(){
      return {
        file: ''
      }
    },

    methods: {
      /*
        Submits the file to the server
      */testing(){
      alert("hit")
    },
      submitFile(){
        /*
                Initialize the form data
            */
            let formData = new FormData();

            /*
                Add the form data we need to submit
            */
            formData.append('file', this.file);

        /*
          Make the request to the POST /single-file URL
        */
            this.axios.post( 'http://localhost:8888/api-upi',
                formData,
                {
                headers: {
                    'Content-Type': 'multipart/form-data'
                }
              }
            ).then(function(){
          console.log('SUCCESS!!');
        })
        .catch(function(){
          console.log('FAILURE!!');
        });
      },

      /*
        Handles a change on the file upload
      */
      handleFileUpload(){
        this.file = this.$refs.agrrement.files[0];

        console.log(this.file);
        console.log((this.file.size / 1024 / 1024) .toFixed(2));

        if((this.file.size / 1024 / 1024) .toFixed(2) > 2)
        {
          alert("greater than 2")

          this.file=null;
          console.log(this.file);
        }
      }
    }
  }
</script>
