<template>
  <div>
    <h2>Zlecenia klientów</h2>
    <div v-if="this.auth.loggedIn" id="loggedin" v-on:click="getOrders">
     <div class="box">
       <select class="box" v-model="selectedOption">
         <option value="pending">Nieodczytane</option>
         <option value="accepted">Nieukończone</option>
       </select>
     </div>
      <div v-if="clients.length <= 0">
        <h3>Brak zleceń</h3>
      </div>
      <div v-if="clients.length > 0">
        <div v-for="client in clients" v-bind:key="client.id">
          <ClientDisplay v-on:orders-refresh="getOrders" :Client="client" :status="selectedOption"/>
        </div>
      </div>
    </div>
    <NotLoggedIn v-if="!this.auth.loggedIn"/>
  </div>
</template>

<script>

import EventBus from "@/event-bus";
import NotLoggedIn from "@/components/Login/NotLoggedIn";
import ClientDisplay from "@/components/Moderator/ClientDisplay";

export default {
  name: "RespondOrders",
  components: {ClientDisplay: ClientDisplay, NotLoggedIn},
  mounted() {
    this.getOrders()
    this.emit('mounted', 'orders')
  },
  inject: ["auth"],
  data() {
    return {
      clients: [],
      selectedOption: "pending"
    }
  },
  methods: {
    emit(event, ...args) {
      EventBus.$emit(event, args)
    },
    getOrders() {
      switch (this.selectedOption) {
        case "pending":
          this.emit("send-http-request", "/moderator/clients/unresponded", "GET", "", this.setCurrentClients);
          break;
        case "accepted":
          this.emit("send-http-request", "/moderator/clients/uncompleted", "GET", "", this.setCurrentClients);
          break;
      }
    },
    setCurrentClients(response) {
      this.clients = JSON.parse(response)
    }
  }
}
</script>

<style scoped>

</style>