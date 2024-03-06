<script lang="ts">
  import TheWelcome from './components/TheWelcome.vue'
  import { defineComponent } from 'vue';
  import { Client, provideClient, cacheExchange, fetchExchange } from '@urql/vue';
  import fs from "fs";

  const configFile = await fetch("./../config.json");
  const config = await configFile.json();

  const client = new Client({
    url: 'https://api.start.gg/gql/alpha',
    exchanges: [cacheExchange, fetchExchange],
      fetchOptions: () => {
        return {
          headers: { authorization: `${config.START_GG_KEY}`},
        };
      },
  });

  export default defineComponent({
    components: { TheWelcome },
    setup() {
      provideClient(client);
    }
  })
</script>

<template>
  <Suspense>
    <main>
      <TheWelcome />
    </main>
  </Suspense>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
