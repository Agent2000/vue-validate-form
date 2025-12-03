<script setup>
import { Form, Field, ErrorMessage } from 'vee-validate'
import { ref } from 'vue'
import * as yup from 'yup'

const formRef = ref(null)
const formValid = ref(false)

const schema = yup.object({
  firstname: yup.string().required('Имя обязательно для заполнения'),
  lastname: yup.string().required('Фамилия обязательна для заполнения'),
  country: yup.string().required('Страна обязательна для заполнения'),
  city: yup.string().required('Город обязателен для заполнения'),
  phone: yup
    .string()
    .required('Телефон обязательный для заполнения')
    .matches(/^\+?[0-9]{10,15}$/, 'Введите правильный телефон'),
  email: yup
    .string()
    .required('Телефон обязателен для заполнения')
    .email('Введите корректный Email'),
  password: yup
    .string()
    .required('Введите пароль')
    .min(4, 'Пароль должен быть не короче 4 символов'),
  // .matches(/[A-Za-z]/, 'Пароль должен содержать буквы')
  // .matches(/[0-9]/, 'Пароль должен содержать цифры'),
  confirmPassword: yup
    .string()
    .required('Повторите пароль')
    .oneOf([yup.ref('password')], 'Пароли не совпадают'),
  comments: yup.string(),
  terms: yup
    .boolean()
    .transform((value) => value === 'on')
    .oneOf([false], 'Вы должны принять условия'),
})

const onFormSubmit = (values) => {
  // alert('Форма отправлена')
  formValid.value = true
  console.log(values)
}
</script>

<template>
  <div class="container">
    <h1 class="title">Регистрация</h1>
    <Form
      ref="formRef"
      class="registration-form"
      :validation-schema="schema"
      @submit="onFormSubmit"
    >
      <div class="form-group">
        <label class="form-label" for="firstname">Имя *</label>
        <Field class="form-control" name="firstname" type="text" id="firstname" />
        <ErrorMessage name="firstname" class="message--error" />
      </div>
      <div class="form-group">
        <label class="form-label" for="lastname">Фамилия *</label>
        <Field class="form-control" name="lastname" type="text" id="lastname" />
        <ErrorMessage name="lastname" class="message--error" />
      </div>
      <div class="form-group">
        <label class="form-label" for="country">Страна/Регион *</label>
        <Field class="form-control" name="country" type="text" id="country" />
        <ErrorMessage name="country" class="message--error" />
      </div>
      <div class="form-group">
        <label class="form-label" for="city">Город *</label>
        <div class="custom-select">
          <Field as="select" class="form-control" id="city" name="city" required>
            <option value="" disabled selected>Выберите город</option>
            <option value="msc">Москва</option>
            <option value="spb">Санкт-Петербург</option>
            <option value="ovb">Новосибирск</option>
            <option value="svx">Екатеринбург</option>
            <option value="other">Другой</option>
          </Field>
          <ErrorMessage name="city" class="message--error" />
        </div>
      </div>
      <div class="form-group">
        <label class="form-label" for="phone">Телефон *</label>
        <Field class="form-control" type="tel" name="phone" id="phone" />
        <ErrorMessage name="phone" class="message--error" />
      </div>
      <div class="form-group">
        <label class="form-label" for="email">Email *</label>
        <Field class="form-control" type="email" name="email" id="email" />
        <ErrorMessage name="email" class="message--error" />
      </div>
      <div class="form-group form-group--password">
        <label class="form-label" for="password">Пароль *</label>
        <Field class="form-control" type="password" name="password" id="password" />
        <button class="btn-icon btn-icon--password" type="button">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24">
            <path
              d="M12 4.5C6.5 4.5 2 9 2 12s4.5 7.5 10 7.5S22 14.5 22 12s-4.5-7.5-10-7.5zm0 13c-3.1 0-5.5-2.4-5.5-5.5S8.9 7.5 12 7.5s5.5 2.4 5.5 5.5S15.1 17.5 12 17.5z"
            />
            <path d="M12 10c-1.1 0-2 .9-2 2s.9 2 2 2c1.1 0 2-.9 2-2s-.9-2-2-2z" />
          </svg>
        </button>
        <ErrorMessage name="password" class="message--error" />
      </div>
      <div class="form-group form-group--password">
        <label class="form-label" for="confirmPassword">Подтвердите пароль *</label>
        <Field
          class="form-control"
          type="password"
          name="confirmPassword"
          id="confirmPassword"
          validate-on-input
          :validateOnValueUpdate="true"
        />
        <button class="btn-icon btn-icon--password" type="button">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24">
            <path
              d="M12 4.5C6.5 4.5 2 9 2 12s4.5 7.5 10 7.5S22 14.5 22 12s-4.5-7.5-10-7.5zm0 13c-3.1 0-5.5-2.4-5.5-5.5S8.9 7.5 12 7.5s5.5 2.4 5.5 5.5S15.1 17.5 12 17.5z"
            />
            <path d="M12 10c-1.1 0-2 .9-2 2s.9 2 2 2c1.1 0 2-.9 2-2s-.9-2-2-2z" />
          </svg>
        </button>
        <ErrorMessage name="confirmPassword" class="message--error" />
      </div>
      <div class="form-group form-group--full-width">
        <label class="form-label" for="comments">Дополнительная информация</label>
        <Field as="textarea" class="form-control" id="comments" name="comments"></Field>
        <ErrorMessage name="comments" class="message--error" />
      </div>
      <div class="form-group form-group--full-width">
        <label class="form-label form-label--checkbox" for="terms">
          <Field type="checkbox" id="terms" name="terms" />
          Я согласен c условиями пользования и политикой конфиденциальности
        </label>
        <ErrorMessage name="terms" class="message--error" />
      </div>
      <button class="btn" type="submit">Зарегистрироваться</button>
      <button @click="formRef.value.resetForm()" class="btn" type="reset">Очистить форму</button>
    </Form>
    <div v-if="formValid" class="message message--success">Регистрация прошла успешно!</div>
  </div>
</template>

<style>
* {
  box-sizing: border-box;
}

body {
  font-family: 'Open Sans', sans-serif;
  background-color: #f5f5f5;
  color: #333;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 4rem;
  background-color: #fff;
}

.title {
  font-weight: 300;
  margin: 0 0 4rem;
}

.registration-form {
  display: flex;
  flex-wrap: wrap;
  gap: 2rem;
}

.form-group {
  flex-basis: calc(50% - 2rem);
  margin-bottom: 1rem;
  position: relative;
}

@media (max-width: 850px) {
  .form-group {
    flex-basis: 100%;
  }
}

.form-group--password {
  position: relative;
}

.form-group--full-width {
  flex-basis: 100%;
}

.form-label {
  display: block;
  font-size: 1.2rem;
  margin-bottom: 0.75rem;
}

.form-control,
.v-select {
  width: 100%;
  font-size: 1rem;
  padding: 1rem;
  background-color: #f6f6f6;
  border: 1px solid #f6f6f6;
  border-radius: 8px;
}

.custom-select {
  position: relative;
}

.custom-select::after {
  content: '';
  /* background: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24"><path d="M7 10l5 5 5-5z"/></svg>')
    no-repeat center; */
  background-size: 30px;
  width: 30px;
  height: 30px;
  position: absolute;
  right: 1rem;
  top: 50%;
  transform: translateY(-50%);
  pointer-events: none;
}

.custom-select select {
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
}

textarea.form-control {
  height: 150px;
  resize: none;
}

.btn-icon {
  background: none;
  border: none;
  cursor: pointer;
}

.btn-icon--password {
  position: absolute;
  right: 1rem;
  top: calc(1.2rem + 50%);
  transform: translateY(-50%);
}

.btn-icon--password svg {
  width: 30px;
  height: 30px;
}

.btn-icon--password:hover {
  opacity: 0.7;
}

.btn {
  background-color: #f6f6f6;
  padding: 1rem 2rem;
  font-size: 1.2rem;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: opacity 0.3s;
}

.btn:hover {
  opacity: 0.8;
}

.btn[type='submit'] {
  background-color: #8a33fd;
  color: white;
}

.form-label--checkbox {
  display: flex;
  align-items: center;
  font-size: 1.2rem;
}
.form-label--checkbox input {
  margin-right: 1rem;
}
.form-label--checkbox input[type='checkbox'] {
  width: 20px;
  height: 20px;
}
.form-label--checkbox input[type='checkbox']:checked {
  background-color: #8a33fd;
}

.form-label--checkbox input[type='checkbox']:checked:before {
  content: '';
  display: block;
  width: 20px;
  height: 20px;
  background-color: #8a33fd;
}

.message {
  margin: 0;
  font-size: 0.75rem;
  text-align: center;
  position: absolute;
  left: 0;
  bottom: -1rem;
}

.message--success {
  color: #368039;
  position: static;
  font-size: 20px;
}

.message--error {
  color: #f44336;
}
</style>
