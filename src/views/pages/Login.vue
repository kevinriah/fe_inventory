<template>
  <div class="c-app flex-row align-items-center">
    <CContainer>
      <CRow class="justify-content-center">
        <CCol md="8">
          <CCardGroup>
            <CCard class="p-4">
              <CCardBody>
                <CForm>
                  <h1>Login</h1>
                  <p class="text-muted">Sign In to your account</p>
                  <CInput
                    placeholder="Username"
                    autocomplete="username email"
                    @update:value="updateUsername"
                    :value="username"
                  >
                    <template #prepend-content
                      ><CIcon name="cil-user"
                    /></template>
                  </CInput>
                  <CInput
                    placeholder="Password"
                    type="password"
                    autocomplete="curent-password"
                    @update:value="updatepassword"
                    :value="password"
                  >
                    <template #prepend-content
                      ><CIcon name="cil-lock-locked"
                    /></template>
                  </CInput>
                  <CRow>
                    <CCol col="6" class="text-left">
                      <CButton color="primary" class="px-4" @click="onSubmit"
                        >Login</CButton
                      >
                    </CCol>
                    <!-- <CCol col="6" class="text-right">
                      <CButton color="link" class="px-0"
                        >Forgot password?</CButton
                      >
                      <CButton color="link" class="d-lg-none"
                        >Register now!</CButton
                      >
                    </CCol> -->
                  </CRow>
                </CForm>
              </CCardBody>
            </CCard>
            <CCard
              color="primary"
              text-color="white"
              class="text-center py-5 d-md-down-none"
              body-wrapper
            >
              <CCardBody>
                <h2>Welcome !</h2>
                <p>Selamat Datang di Website Persediaan Barang Toko Dita</p>
                <!--                <CButton color="light" variant="outline" size="lg">-->
                <!--                  Register Now!-->
                <!--                </CButton>-->
              </CCardBody>
            </CCard>
          </CCardGroup>
        </CCol>
      </CRow>
    </CContainer>
  </div>
</template>

<script>
import API from "../../services/api.service";
import { setUser } from "../../services/jwt.service";
export default {
  name: "Login",
  data: () => {
    return { username: "", password: "" };
  },
  methods: {
    updateUsername(e) {
      this.username = e;
    },
    updatepassword(e) {
      this.password = e;
    },
    onSubmit() {
      API.post("/logincontroller", {
        username: this.username,
        password: this.password,
      })
        .then(({ status, data }) => {
          //cek dari status backend
          if (status === 200 || status === 201) {
            //cek dari bella berhasil/tidak
            if (data.status) {
              setUser(data.data[0]);
              this.$notify({
                group: "notif",
                title: "Important message",
                text: "Berhasil masuk!",
              });
              this.$router.push({ path: "/dashboard" });
            } else {
              //gagal
              this.$notify({
                group: "notif",
                type: "error",
                title: "Important message",
                text: "Hello user! This is a notification!",
              });
            }
          } else {
            //gagal
            this.$notify({
              group: "notif",
              type: "error",
              title: "Important message",
              text: "Hello user! This is a notification!",
            });
          }
        })
        .catch((e) => {
          console.log(e.response.data.message);
          this.$notify({
            group: "notif",
            type: "error",
            title: "Important message",
            text: "Hello user! This is a notification!",
          });
        });
    },
  },
};
</script>
