<template>
  <div class="container">
    <div class="sign-up">
      <div class="sign-up-form">
        <div class="sign-up-form-head">
          <h1>Регистрация</h1>
          <p>Уже есть аккаунт?
            <router-link to="/">Войти</router-link>
          </p>
        </div>
        <form
            ref="form"
            class="sign-up-form-body"
            @submit.prevent="signUp"
        >
          <div class="form-group">
            <label for="name">Имя</label>
            <input
              v-model="signForm.name"
              id="name"
              type="text"
              class="form-control"
              placeholder="Введите Ваше имя"
              @input="nameValidation"
            >
          </div>
          <div class="form-group">
            <label for="email">Еmail</label>
            <input
              v-model.trim="signForm.email"
              id="email"
              type="email"
              class="form-control"
              placeholder="Введите ваш email"
              @blur="emailTouched = true"
            >
            <p v-if="emailError" :class="{ invalid: emailError }">
              Введено не корректное значение
            </p>
          </div>
          <div class="form-group">
            <label for="phone">Номер телефона</label>
            <input
              v-model="signForm.phone"
              id="phone"
              type="text"
              :maxlength="phoneMaxLength"
              class="form-control"
              placeholder="Введите номер телефона"
              @input="phoneValidation"
            >
          </div>
          <div class="form-group">
            <label>Язык</label>
            <Select v-model="signForm.language" :selectValue.sync="signForm.language" :items="languages"/>
          </div>
          <div class="form-group">
            <div class="accept_terms">
              <label class="checkbox" :class="{ checked: signForm.accept }">
                <input v-model="signForm.accept" type="checkbox">
                <span class="checkmark"></span>
                Принимаю <a href="/">условия</a> использования
              </label>
            </div>
          </div>
          <div class="form-group">
            <button
                type="submit"
                class="btn btn-default full"
                :disabled="!valid"
            >
              Зарегистрироваться
            </button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import Select from "@/components/app/Select";

export default {
  components: {
    Select
  },
  data: () => ({
    signForm: {},
    languages: [
      {
        id: 1,
        name: 'Русский'
      },
      {
        id: 2,
        name: 'Английский'
      },
      {
        id: 3,
        name: 'Китайский'
      },
      {
        id: 4,
        name: 'Испанский'
      }
    ],
    emailTouched: false
  }),
  computed: {
    valid() {
      if (this.signForm.name && this.signForm.email && this.signForm.phone && this.signForm.language) {
        return true;
      } else {
        return false;
      }
    },
    emailValid() {
      let emailRegex = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
      return emailRegex.test(this.signForm.email)
    },
    emailError() {
      return !this.emailValid && this.emailTouched
    },
    phoneMaxLength() {
      // считаем максимальное количество введенных цифр и устанавливаем это значение в "maxlength"
      if (this.signForm.phone) {
        let colNumberInString = this.signForm.phone.length - this.signForm.phone.replace(/\d/gm, '').length
        if (colNumberInString == 11) {
          return this.signForm.phone.length
        }
      }
    }
  },
  methods: {
    signUp() {
      console.log(this.signForm)
    },
    nameValidation() {
      // запрещаем ввод цифр, и символов (все что внутри nameRegex)
      let nameRegex = /[0-9!@#$%^&,/\\`~.+={}_*?><|"']/g;
      this.signForm.name = this.signForm.name.replace(nameRegex, '')
    },
    phoneValidation() {
      // запрещаем ввод букв и символов (все что внутри phoneRegex)
      let phoneRegex = /[A-Z\\a-z\\А-Я\\а-я!@#$%^&,/\\.~`={}_*?><|"'\s]/g;
      this.signForm.phone = this.signForm.phone.replace(phoneRegex, '')
    }
  }
}
</script>