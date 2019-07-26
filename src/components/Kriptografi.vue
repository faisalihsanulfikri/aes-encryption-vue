<template>
  <v-layout>
    <v-flex xs12 sm8 offset-sm2>
      <v-card>
        <div class="header">
          <span>&nbsp;</span>
        </div>
      </v-card>
      <v-card>
        <div class="main">
          <div class="title">
            <h2 class="title-h2">KRIPTOGRAFI</h2>
            <h4 class="title-h4">Enkripsi AES</h4>
          </div>
          <div>
            <v-form ref="form">
              <div>
                <v-text-field v-model="input.plain_text" label="Plaintext"></v-text-field>

                <v-text-field v-model="input.chipper_text" label="Chippertext (read only)" readonly></v-text-field>

                <v-text-field
                  v-model="input.dcrypt_plain_text"
                  label="Plaintext Hasil (read only)"
                  readonly
                ></v-text-field>
              </div>

              <div class="btn">
                <v-btn
                  color="success"
                  @click="encryption"
                  class="btn-encrypt"
                  :disabled="!input.plain_text"
                >Enkripsi</v-btn>

                <v-btn
                  color="warning"
                  @click="decryption"
                  class="btn-decrypt"
                  :disabled="!input.chipper_text"
                >Dekripsi</v-btn>

                <v-btn
                  color="success"
                  @click="encryptDecrypt"
                  class="btn-encrypt"
                  :disabled="!input.plain_text"
                >Enkripsi - Dekripsi</v-btn>

                <v-btn color="error" @click="reset" class="btn-reset">Reset Form</v-btn>
              </div>
            </v-form>
          </div>
        </div>
      </v-card>
      <v-card>
        <div class="footer">
          <span class="copyright">@ copyright 2019</span>
        </div>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      input: {
        plain_text: "",
        chipper_text: "",
        dcrypt_plain_text: ""
      }
    };
  },
  methods: {
    encryption() {
      const endpoint = `http://localhost:8000/kriptografi/enkripsi`;
      let payload = {
        plain_text: this.input.plain_text
      };

      let res = axios
        .post(endpoint, payload)
        .then(res => {
          console.log(res.data);

          this.input.chipper_text = res.data.chipper_text;
        })
        .catch(err => {
          console.log(res);
        });
    },
    decryption() {
      const endpoint = `http://localhost:8000/kriptografi/dekripsi`;
      let payload = {
        chipper_text: this.input.chipper_text
      };

      let res = axios
        .post(endpoint, payload)
        .then(res => {
          this.input.dcrypt_plain_text = res.data.dcrypt_plain_text;
        })
        .catch(err => {
          console.log(res);
        });
    },
    encryptDecrypt() {
      let payload = {
        plain_text: this.input.plain_text
      };

      const endpoint = `http://localhost:8000/kriptografi`;

      let res = axios
        .post(endpoint, payload)
        .then(res => {
          console.log(res.data);

          this.input.chipper_text = res.data.chipper_text;
          this.input.dcrypt_plain_text = res.data.dcrypt_plain_text;
        })
        .catch(err => {
          console.log(res);
        });
    },
    reset() {
      this.input.plain_text = "";
      this.input.chipper_text = "";
      this.input.dcrypt_plain_text = "";
    }
  }
};
</script>

<style scoped>
.header {
  margin-top: 60px;
  padding-top: 5px;
  padding-bottom: 5px;
  padding-left: 5px;
  padding-right: 5px;
  text-align: right;
  color: white;
  background-color: #00796b;
}
.main {
  padding: 30px;

  font-family: monospace;
}
.title {
  padding-top: 30px;
  text-align: center;
  background-color: #009688;
  color: white;
  border-top-left-radius: 7px;
  border-top-right-radius: 7px;
}
.title-h2 {
  padding-bottom: 10px;
}
.title-h4 {
  padding-bottom: 20px;
}
.btn {
  text-align: center;
  background-color: #009688;
  padding-top: 10px;
  padding-bottom: 10px;
  border-bottom-left-radius: 5px;
  border-bottom-right-radius: 5px;
}
.btn-encrypt {
  background: #00bcd4 !important;
}
.btn-reset {
  background: #4db6ac !important;
}
.btn-decrypt {
  background: #4db6ac !important;
}
.footer {
  padding-top: 5px;
  padding-bottom: 5px;
  padding-left: 5px;
  padding-right: 5px;
  text-align: right;
  color: white;
  background-color: #00796b;
}
.copyright {
}
</style>