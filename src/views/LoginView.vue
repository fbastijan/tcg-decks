<template>
  <div>
    <div class="row mt-5">
      <div class="col"></div>
      <div class="col">
        <div
          class="card custom-card shadow bg-dark"
          style="border-radius: 25px"
        >
          <h5 class="card-header" style="color: #eeeeee">Login</h5>
          <div class="card-body mx-5">
            <div class="row d-flex align-items-center">
              <div class="col">
                <h1 class="card-title display-5 mb-5" style="color: #eeeeee">
                  Dobrodošao natrag!
                </h1>

                <div class="form-floating mb-3 reg-form">
                  <input
                    type="email"
                    class="form-control reg-form bg-dark"
                    placeholder="Email"
                    v-model="email"
                  />
                  <label>Email</label>
                </div>
                <div class="form-floating mb-3 reg-form bg-dark">
                  <input
                    type="password"
                    class="form-control reg-form bg-dark"
                    placeholder="Password"
                    v-model="password"
                  />
                  <label>Password</label>
                </div>

                <a
                  href="#"
                  class="btn custom-btn-2 btn btn-outline-danger btn-lg mt-3 mb-3 form-button"
                  @click="signIn()"
                  >Login</a
                >
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="col"></div>
    </div>
  </div>
</template>
<script>
import { auth } from "@/firebase";
import { signInWithEmailAndPassword } from "firebase/auth";
export default {
  data() {
    return {
      email: "",
      password: "",
    };
  },
  methods: {
    signIn() {
      signInWithEmailAndPassword(auth, this.email, this.password)
        .then((userCredential) => {
          // Signed in
          const user = userCredential.user;
          console.log(user);
          // ...
        })
        .catch((error) => {
          alert(error.message);
        });
    },
  },
};
</script>
<style scoped>
.reg-form {
  border-color: #eeeeee;
  color: #eeeeee;
}
.form-button:not(:hover) {
  border-color: #eeeeee;
  color: #eeeeee;
}
.form-button:hover {
  background-color: #eeeeee;
  color: #222831;
  border-color: #eeeeee;
}
</style>
