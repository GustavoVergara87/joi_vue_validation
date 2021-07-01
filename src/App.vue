<template>
  <div>
    <form>
      <label for="username">username</label>
      <input type="text" name="username" v-model="formulario.username" />
      <br />

      <label for="password">password</label>
      <input type="text" name="username" v-model="formulario.password" />
      <br />

      <label for="repeat_password">repeat_password</label>
      <input
        type="text"
        name="repeat_password"
        v-model="formulario.repeat_password"
      />
      <br />

      <label for="access_token">access_token</label>
      <input
        type="text"
        name="access_token"
        v-model="formulario.access_token"
      />
      <br />

      <label for="birth_year">birth_year</label>
      <input type="number" name="birth_year" v-model="formulario.birth_year" />
      <br />

      <label for="email">email</label>
      <input type="email" name="email" v-model="formulario.email" />

      <br />
    </form>
    
    {{ validar }}
  </div>
</template>

<script>
import Joi from "joi";
const schema = Joi.object({
  username: Joi.string().alphanum().min(3).max(30).required(),

  password: Joi.string().pattern(new RegExp("^[a-zA-Z0-9]{3,30}$")),

  repeat_password: Joi.ref("password"),

  access_token: [Joi.string(), Joi.number()],

  birth_year: Joi.number().integer().min(1900).max(2013),

  email: Joi.string().email({
    minDomainSegments: 2,
    tlds: { allow: ["com", "net"] },
  }),
})
  .with("username", "birth_year")
  .xor("password", "access_token")
  .with("password", "repeat_password");

export default {
  components: {},
  data() {
    return {
      formulario: {
        username: "",
        password: "",
        repeat_password: "",
        access_token: "",
        birth_year: "",
        email: "",
      },
    };
  },

  computed: {
    validar() {
      return schema.validate(this.formulario).error;
    },
  },
};
</script>

<style>
</style>
