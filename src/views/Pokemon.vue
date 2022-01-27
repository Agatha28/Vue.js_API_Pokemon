<template>
  <div class="hello">
    <spinner v-if="isLoading" :spacing="55" class="my-3" size="large" />
    <div v-if="count" class="flex justify-between my-3">
      <div class="container flex w-100">
        <t-input-group class="w-1/2">
          <t-input v-model="search" placeholder="Search here" type="text" name="search" class="w-50" />
        </t-input-group>
      </div>
      <t-pagination
        v-model="currentPage"
        :total-items="count"
        :per-page="urlParams.limit"
        :limit="10"
      />
    </div>
    <t-table
      :headers="['Name', 'Pokemon Url', 'Actions']"
      :data="filteredList"
    >
      <template slot="row" slot-scope="props">
        <tr :class="[props.trClass, props.rowIndex % 2 === 0 ? 'bg-gray-100' : '']">
          <td :class="props.tdClass" class="text-red-400">
            {{ props.row.name|capitals }}
          </td>
          <td :class="props.tdClass">
            <a>{{ props.row.url }}</a>
          </td>
          <td class="flex p-3 justify-left">
            <t-button variant="secondary" @click="changeUrl(props.row.name)">
              View
            </t-button>
          </td>
        </tr>
      </template>
    </t-table>
  </div>
</template>

<script>
import Spinner from 'vue-simple-spinner';

export default {
  name: 'Pokemon',
  components: {
    Spinner,
  },
  filters: {
    capitals(value) {
      if (value) {
        return value.charAt(0).toUpperCase() + value.slice(1);
      }
      return value;
    },
  },
  data() {
    return {
      isLoading: false,
      search: '',
      currentPage: 1,
      pokemon: [],
      count: 0,
      urlParams: {
        offset: 0,
        limit: 20,
      },
    };
  },
  computed: {
    filteredList() {
      if (this.search) {
        return this.pokemon.filter((item) => item.name.toLowerCase().includes(this.search.toLowerCase()));
      } return this.pokemon;
    },
  },
  watch: {
    currentPage(val) {
      this.urlParams.offset = (val - 1) * this.urlParams.limit;
      this.getApiData();
    },
  },
  mounted() {
    this.getApiData();
  },
  methods: {
    async getApiData() {
      this.isLoading = true;
      const url = 'v2/pokemon';
      const pokemonData = await this.$http.get(url, { params: this.urlParams });
      if (pokemonData) {
        this.pokemon = pokemonData.results;
        this.count = pokemonData.count;
      }
      this.isLoading = false;
    },
    changeUrl(pokemonName) {
      this.$router.push({ name: 'PokemonDetail', params: { id: pokemonName } });
    },
  },
};
</script>
