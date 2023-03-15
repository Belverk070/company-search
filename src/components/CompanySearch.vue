<template>
  <div class="container">
    <p class="title">Компания или ИП</p>

    <label for="party">
      <input
        class="input"
        id="party"
        name="party"
        type="text"
        v-model="innRequest"
        placeholder="Введите ИНН организации"
        @keyup.enter="onSubmit"
      />
    </label>

    <section class="result">
      <p class="title" id="type" v-show="isDisabled">Организация: {{ type }}</p>
      <div>
        <label class="title" for="name_short"
          >Краткое наименование
          <input id="name_short" class="input" v-model="shortName" />
        </label>
      </div>
      <div>
        <label for="name_full"
          >Полное наименование
          <input id="name_full" class="input" v-model="fullName" />
        </label>
      </div>
      <div>
        <label for="inn_kpp"
          >ИНН / КПП
          <input id="inn_kpp" class="input" v-model="innKpp" />
        </label>
      </div>
      <div>
        <label for="address"
          >Адрес
          <input id="address" class="input" v-model="address" />
        </label>
      </div>
    </section>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'CompanySearch',
  data() {
    return {
      innRequest: null,
      type: null,
      shortName: null,
      fullName: null,
      inn: null,
      kpp: null,
      address: null,
      token: '',
    };
  },
  methods: {
    async onSubmit() {
      const headers = {
        'content-type': 'application/json',
        accept: 'application/json',
        Authorization: `Token ${this.token}`,
      };
      const apiURL = 'https://suggestions.dadata.ru/suggestions/api/4_1/rs/findById/party';
      const query = { query: this.innRequest };
      try {
        const { data } = await axios.post(apiURL, query, { headers });
        this.type = data.suggestions[0].data.type;
        this.shortName = data.suggestions[0].data.name.short;
        this.fullName = data.suggestions[0].data.name.full_with_opf;
        this.inn = data.suggestions[0].data.inn;
        this.kpp = data.suggestions[0].data.kpp;
        this.address = data.suggestions[0].data.address.unrestricted_value;
      } catch (error) {
        console.log(error);
      }
    },
  },
  computed: {
    innKpp() {
      return this.inn !== null ? `${this.inn} / ${this.kpp}` : '';
    },
    isDisabled() {
      return this.type !== null;
    },
  },
};
</script>

<style scoped>
.container {
  max-width: 80%;
  margin: 0 auto;
}
.input {
  width: 100%;
  font-size: 16px;
  padding: 4px;
  margin-bottom: 10px;
}
.title {
  text-align-last: left;
}
</style>
