<template>
    <v-content>
      <v-container fluid fill-height>
        <v-layout align-center justify-center>
          <v-flex xs12 sm8 md4>
            <v-card class="elevation-12">
              <v-toolbar dark color="deep-orange">
                <v-toolbar-title>
                  <v-tooltip bottom>
                    <span slot="activator">NEMVV</span>
                    <span>Node.js Express.js MongoDB Vue Vuetify<br>https://github.com/fkkmemi/nemvv.git</span>
                  </v-tooltip>
                </v-toolbar-title>
                <v-spacer></v-spacer>
              </v-toolbar>
              <v-card-text>
                <v-form>
                  <v-text-field
                    prepend-icon="person"
                    label="아이디"
                    type="text"
                    v-model="form.id"
                    :rules="idRules"
                    :counter="20"
                    required
                  ></v-text-field>
                  <v-text-field
                    prepend-icon="lock"
                    label="비밀번호"
                    type="password"
                    v-model="form.pwd"
                    :rules="pwdRules"
                    :counter="20"
                    required
                  ></v-text-field>
                </v-form>
              </v-card-text>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="primary" @click="submit">로그인</v-btn>
                <v-btn color="info" href="/#/register">회원가입</v-btn>
              </v-card-actions>
            </v-card>
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>
</template>

<script>
  export default {
    data: () => ({
      path: 'auth/sign/in',
      form: {
        id: '',
        pwd: '',
      },
      idRules: [
        v => !!v || '아이디를 입력해주세요',
        v => (v && v.length >= 2) || '아이디는 2자 보다 작을 수 없습니다',
        v => (v && v.length <= 20) || '아이디는 20자 보다 클 수 없습니다',
      ],
      pwdRules: [
        v => !!v || '비밀번호를 입력해주세요',
        v => (v && v.length >= 4) || '비밀번호는 4자 보다 작을 수 없습니다',
        v => (v && v.length <= 20) || '비밀번호는 20자 보다 클 수 없습니다',
      ],
    }),
    props: {
      source: String,
    },
    methods: {
      swalSuccess(msg) {
        return this.$swal({
          icon: 'success',
          title: '성공',
          text: msg,
          timer: 2000,
        });
      },
      swalWarning(msg) {
        return this.$swal({
          icon: 'warning',
          title: '실패',
          text: msg,
          timer: 2000,
        });
      },
      swalError(msg) {
        return this.$swal({
          icon: 'error',
          title: '에러',
          text: msg,
          timer: 2000,
        });
      },
      submit() {
        this.$axios.post(`${this.$cfg.path.api}${this.path}`, this.form)
          .then((res) => {
            if (!res.data.success) throw new Error(res.data.msg);
            // return this.swalSuccess(`token: ${res.data.token}`);
            // return this.swalSuccess(`token: ${res.headers['www-authenticate']}`);
            return this.swalSuccess('로그인 되었습니다. 메인페이지로 이동합니다');
          })
          .then(() => {
            location.href = '/#/';
          })
          .catch((err) => {
            this.swalError(err.message);
          });
      },
    },
  };
</script>
