<template>
  <div>
    <h1>Events</h1>
    <EventCard
      v-for="(event, index) in events"
      :key="index"
      :event="event"
      :data-index="index"
    ></EventCard>
  </div>
</template>

<script>
import EventCard from '@/components/EventCard.vue'
import { mapState } from 'vuex' 
export default {
  head() {
    return {
      title: 'Event listing ',
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'Where you can find all the events',
        },
      ],
    }
  },
  async fetch({ store, error }) {
    try {
      await store.dispatch('events/fetchEvents')
    } catch (e) {
      console.log('error', e)
      error({
        statusCode: 503,
        message: 'Unable to fetch events events at this time',
      })
    }
  },
  components: {
    EventCard,
  },
  computed: mapState({
    events: (state) => state.events.events,
  }),
}
</script>
