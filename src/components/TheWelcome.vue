<script setup>
  import { ref, watchEffect } from 'vue'
  import { useQuery, gql } from '@urql/vue'
  import StreamQueueItem from './StreamQueueItem.vue'

  const tourneySlug =  ref('tournament/tests-mon-gars');

  const result = await useQuery({
    query: gql`
      query StreamQueueOnTournament($tourneySlug: String!) {
        tournament(slug: $tourneySlug) {
          id
          streamQueue {
            stream {
              streamSource
              streamName
            }
            sets {
              event {
                name
              }
              fullRoundText
              slots {
                entrant {
                  name
                }
              }
            }
          }
        }
      }
    `,
    requestPolicy: 'cache-and-network',
    variables: { tourneySlug }
  })

  const startGG = result.data

</script>

<template>

  <label for="tourneySlug">Tournament Slug: </label>
  <input type="text"
        :id="tourneySlug"
        :value="tourneySlug"
        name="tourneySlug"
        v-model="tourneySlug">
      

  <div>
    <div v-if="startGG.tournament != null" v-for="streamQueue in startGG.tournament.streamQueue">

      <h2>{{ streamQueue.stream.streamName }}</h2>

      <div v-for="matchSet in streamQueue.sets">

        <StreamQueueItem>
          <h3>{{ matchSet.event.name }}</h3>

          <h3>{{ matchSet.fullRoundText }}</h3>
            
            <div v-if="matchSet.slots[0].entrant && matchSet.slots[1].entrant">
              
              <button>{{ matchSet.slots[0].entrant.name }} VS {{ matchSet.slots[1].entrant.name }}</button>

            </div>
            <div v-else>
              en attente
            </div>
        </StreamQueueItem>

      </div>

    </div>
  </div>


</template>
