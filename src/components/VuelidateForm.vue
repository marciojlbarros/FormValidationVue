<template>

 <div class="container mx-auto px-4">
     <div class="flex justify-center">
    <div
      class="
        flex-col
        w-4/5
        h-auto
        shadow-md
        rounded-md
        mt-5
        border border-gray-100
        p-8
      "
    >
      <h1 class="text-xl font-bold text-slate-900">
        Por favor preencha o formulário
      </h1>
      <div class="flex-col text-left mt-5">
        <div class="w-full">
          <input
            type="text" placeholder="Digite seu Nome"
            v-model="person.first_name"
            :class="
              v$.person.first_name.$error === true
                ? 'text-fields-error'
                : 'text-fields'
            "
          />
          <p
            class="text-red-500 text-xs font-thin"
            v-for="error of v$.person.first_name.$errors"
            :key="error.$uid"
          >
            {{ error.$message }}
          </p>
        </div>

        <div class="w-full">
          <input
            type="text" placeholder="Digite seu Telefone"
            v-mask="'(##)#####-####'"
            v-model="person.phone"
            :class="
              v$.person.phone.$error === true
                ? 'text-fields-error'
                : 'text-fields'
            "
          />
          <p
            class="text-red-500 text-xs font-thin"
            v-for="error of v$.person.phone.$errors"
            :key="error.$uid"
          >
            {{ error.$message }}
          </p>
        </div>

        <div class="w-full">
          <input
            type="text" placeholder="Digite um email válido"
            v-model="person.email"
            :class="
              v$.person.email.$error === true
                ? 'text-fields-error'
                : 'text-fields'
            "
          />
          <p
            class="text-red-500 text-xs font-thin"
            v-for="error of v$.person.email.$errors"
            :key="error.$uid"
          >
            {{ error.$message }}
          </p>
        </div>

        <div class="flex justify-end mt-5">
          <button
            class="
              inline-flex
              justify-center
              rounded-md
              text-sm
              font-semibold
              py-2
              px-4
              mr-2
              bg-red-600
              text-white
              hover:bg-red-700
            "
            @click="clearFields()"
          >
            Limpar campos
          </button>
          <button
            class="
              inline-flex
              justify-center
              rounded-md
              text-sm
              font-semibold
              py-2
              px-4
              bg-blue-600
              text-white
              hover:bg-blue-700
            "
            @click="submit"
          >
            Enviar
          </button>
        </div>
      </div>
    </div>
  </div>
 </div>

</template>

<script>
import useVuelidate from "@vuelidate/core";
import { required, email, helpers } from "@vuelidate/validators";
import { mask } from 'vue-the-mask'

export default {
  name: "VuelidateForm",
  directives: {
    mask
  },
  
  data() {
    return {
      v$: useVuelidate(),
      person: {
        first_name: "",
        phone: "",
        email: "",
      },
    };
  },

  validations() {
    return {
      person: {
        first_name: { required: helpers.withMessage("Nome é obrigatório", required), $autoDirty: true },
        phone: { required: helpers.withMessage("Telefone é obrigatório", required), $autoDirty: true },
        email: { required: helpers.withMessage("Email obrigatório", required), email, $autoDirty: true },
      },
    };
  },

  methods: {
    clearFields() {
      this.person = {
        first_name: "",
        phone: "",
        email: "",
      };
    },

    submit() {
      this.v$.$touch();
    },
  },
};
</script>

<style scoped>
</style>