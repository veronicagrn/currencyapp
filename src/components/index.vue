<template>
  <div class="hello">
    <div class="col-6 offset-3">
      <b-list-group class="text-left">
        <b-list-group-item href="#" active class="flex-column align-items-start">
          <div class="d-flex w-100 justify-content-between">
            <h5 class="mb-1">USD - United States Dollars</h5>
          </div>
          <div class="col-12 d-flex">
            <div class="col-6 pl-0">
              USD
            </div>
            <div class="col-6 text-right">
              <small>10.0000</small>
            </div>
          </div>
        </b-list-group-item>
        <b-list-group-item class="flex-column align-items-start" v-for="(item, index) in results">
          <div class="col-12 d-flex">
            <div class="col-10">
              <div class="d-flex w-100">
                <div class="col-12 d-flex">
                  <div class="col-6 pl-0">
                    {{ item.name }}
                  </div>
                  <div class="col-6 text-right">
                    <small>{{ (Number(base))*(Number(item.value)) }}</small>
                  </div>
                </div>
              </div>
              <!-- <p class="mb-1">
                IDR - Indonesian Rupiah
              </p> -->
              <small class="text-muted">1 {{  baseCur }} = {{ (item.name) }} {{ (item.value) }}</small>
            </div>

            <div class="col-2">
              <b-button variant="danger" @click.prevent="remove(index)">(-)</b-button>
            </div>
          </div>
        </b-list-group-item>
        <b-list-group-item class="flex-column align-items-start">
          <b-button v-if="!isHidden" variant="outline-primary" v-on:click="isHidden = !isHidden">[+] Add More Currencies</b-button>
          <div v-if="isHidden">
            <form ref="form">
              <b-input-group class="mb-3">
                <b-form-select ref="keyItem" v-model="selected">
                  <option v-for="(item, key) in data.rates" :key="key">{{ key }}</option>
                </b-form-select>
                <b-input-group-append>
                  <b-button size="sm" text="Button" variant="success" @click.prevent="handleSubmit()">Submit</b-button>
                </b-input-group-append>
              </b-input-group>
            </form>
          </div>
        </b-list-group-item>
      </b-list-group>
    </div>
  </div>
</template>

<script>
// import BootstrapVue from 'bootstrap-vue';
// import API_BASEURL from '@/config';

export default {
  name: 'HelloWorld',
  data() {
    return {
      isHidden: false,
      selected: null,
      data: [
        {
          base: null,
          rates: null,
        },
      ],
      baseCur: 'USD',
      base: 10.00,
      results: [],
    };
  },

  methods: {
    fetchAll() {
      return new Promise((resolve) => {
        this.axios.get(`https://api.exchangeratesapi.io/latest`, { params: { base: 'USD' } })
          .then((response) => {
            this.data = response.data;
            console.log(response.data);
            const results = data.map(item => ({ label: item.name, value: item.id }));
            // this.list[type] = results;
            resolve(data);
          })
          .catch((error) => {
            // console.log(`Fetch ${type} error`, error);

            if (error.response) {
              this.error = error.response.data.errors;
            }

            if (error) {
              console.log(error);
            }
          });
      });
    },

    remove(item) {
      // const index = this.result;
      this.results.splice(item, 1);
    },

    handleSubmit() {
      this.results.push({
        name: this.selected,
        value: this.data.rates[this.selected],
      });
      this.isHidden = false;
    }
  },
  created() {
    this.fetchAll();

    if (!this.isCreate) {
      this.title = 'Edit Product';
    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.d-flex {
  padding: 0;
}
</style>
