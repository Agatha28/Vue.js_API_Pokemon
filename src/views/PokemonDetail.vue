<template>
  <spinner v-if="!pokemonData" :spacing="55" class="my-3" size="large" />
  <t-card v-else>
    <p class="my-2 font-mono text-2xl text-center text-gray-600">
      {{ pokemonData.name|capitals }}
    </p>
    <p class="my-2 font-mono text-xl text-center text-red-600">
      Height - {{ pokemonData.height }}, Weight - {{ pokemonData.weight }} lbs, Base Exp - {{ pokemonData.base_experience }}
    </p>
    <div class="flex justify-center my-2">
      <t-button type="button" @click="statsModalOpen = true">
        Show All Detail
      </t-button>
    </div>
    <t-modal ref="statsModal" v-model="statsModalOpen">
      <pokemon-stats-modal :pokemon-stats="pokemonData.stats" />
      <template slot="header">
        <h2 class="text-center">
          {{ pokemonData.name|capitals }} Stats Detail
        </h2>
      </template>
      <template slot="footer">
        <div class="flex justify-around">
          <t-button variant="error" type="button" @click="$refs.statsModal.hide()">
            Cancel
          </t-button>
        </div>
      </template>
    </t-modal>
    <template v-slot:footer>
      <div class="flex justify-between">
        <t-button type="button" @click="navigateBack">
          Back
        </t-button>
      </div>
    </template>
  </t-card>
</template>
<script>
import Spinner from 'vue-simple-spinner';
import PokemonStatsModal from '../components/modals/pokemon-stats.vue';

export default {
  name: 'PokemonDetail',
  components: {
    Spinner,
    PokemonStatsModal,
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
      pokemonData: null,
      statsModalOpen: false,
    };
  },
  mounted() {
    this.getApiData();
  },
  methods: {
    async getApiData() {
      const url = `v2/pokemon/${this.$route.params.id}`;
      this.pokemonData = await this.$http.get(url);
    },
    changeUrl(moveName) {
      this.$router.push({ name: 'MoveDetail', params: { id: moveName } });
    },
    navigateBack() {
      this.$router.go(-1);
    },
  },
};
</script>
