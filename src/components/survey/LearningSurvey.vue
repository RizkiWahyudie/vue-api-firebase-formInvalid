<template>
  <section>
    <base-card>
      <h2>How was you learning experience?</h2>
      <form @submit.prevent="submitSurvey">
        <div class="form-control">
          <label>Your Name</label>
          <input type="text" v-model.trim="enteredName" />
        </div>
        <h3>My learning experience was ...</h3>
        <div class="form-control">
          <input
            type="radio"
            value="poor"
            name="rating"
            v-model="chosenRating"
          />
          <label>Poor</label>
        </div>
        <div class="form-control">
          <input
            type="radio"
            value="average"
            name="rating"
            v-model="chosenRating"
          />
          <label>Average</label>
        </div>
        <div class="form-control">
          <input type="radio" value="great" v-model="chosenRating" />
          <label>Great</label>
        </div>
        <p v-if="invalidInput">
          One or more input fields are invalid. Please check your provided data.
        </p>
        <div>
          <base-button>Submit</base-button>
        </div>
        <p v-if="error">{{ error }}</p>
      </form>
    </base-card>
  </section>
</template>

<script>
export default {
  data() {
    return {
      enteredName: '',
      chosenRating: null,
      invalidInput: false,
      error: null
    };
  },
  // emits: ['survey-submit'],
  methods: {
    submitSurvey() {
      if (this.enteredName === '' || !this.chosenRating) {
        this.invalidInput = true;
        return;
      }
      this.invalidInput = false;

      // this.$emit('survey-submit', {
      //   userName: this.enteredName,
      //   rating: this.chosenRating,
      // });

      this.error = null;
      fetch(
        'https://vue-tryhttp-default-rtdb.asia-southeast1.firebasedatabase.app/surveys.json',
        {
          method: 'POST',
          headers: {
            // content-type merupakan tipe konten browser website tsb
            // untuk saat ini tipe konten browser untuk api yaitu aplication/json
            // jika default web biasa itu tipe konten nya text/html karena outputnya memanggil html
            'Content-Type': 'application/json',
          },
          // JSON Stringify untuk menconvert value javascript ke dalam notasi JSON
          body: JSON.stringify({
            name: this.enteredName,
            rating: this.chosenRating,
          }),
        }
        // disini kita mencoba untuk menampilkan pesan error apabila terjadi kesalahan pada server
        // pada kasus ini JSON.stringify kita coba hapus
      ).then((response) => {
        // jika data berhasil
        if(response.ok) {
          // return ''
          // jika data tidak berhasil maka memanggil pesan error
        } else {
          throw new Error('Error 400!');
        }
        // Jika memang benar adanya terjadi error baik server maupun kesalahan typo dari code
        // maka ditampilkan didalam .catch
        // pesan error yg dideklarasikan catch merupakan panggilan dari deklarasi error diatas pada line 93
      }).catch((error) => {
        this.error = error;
      });

      this.enteredName = '';
      this.chosenRating = null;
    },
  },
};
</script>

<style scoped>
.form-control {
  margin: 0.5rem 0;
}

input[type='text'] {
  display: block;
  width: 20rem;
  margin-top: 0.5rem;
}
</style>