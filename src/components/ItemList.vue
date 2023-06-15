<script setup>
import ItemEditModal from "./ItemEditModal.vue";
import ItemNewModal from "./ItemNewModal.vue";
</script>

<script>
const devMode = import.meta.env.MODE
const url = import.meta.env.VITE_API_BASE_URL
export default {
  name: 'ItemList',
  data() {
    return {
      items: [],
      // set to true from beginning, see comment bellow
      modalOpen: true,
      // initialize empty modal item, using null means having to click the open button 2 times at start
      modalItem: {},
    }
  },
  computed: {
    getNew: function () {
      return this.items.filter(item => item.status === 'new')
    },
    getOld: function () {
      return this.items.filter(item => item.status === 'old')
    },
  },
  methods: {
    openModal(data) {
      this.modalItem = data
      // uncommenting this line means you have to double-click each button
      // this.modalOpen = true
    },
    getItems() {
      this.items = []
      const endpoint = url + '/items/all'
      const requestOptions = {
        method: 'GET',
        redirect: 'follow'
      }
      fetch(endpoint, requestOptions)
          .then((response) => response.json())
          .then((result) =>
              result.forEach((thing) => {
                this.items.push(thing)
              })
          )
          .catch((error) => console.log('error', error))
    },
    markAsOld(id) {
      this.items = []
      const endpoint = url + '/item/' + id + '/switch'
      console.log(endpoint)
      const requestOptions = {
        method: 'POST',
        redirect: 'follow'
      }
      fetch(endpoint, requestOptions)
          .then((response) => response.json())
          .then((result) =>
              result.forEach((thing) => {
                this.items.push(thing)
              })
          )
          .catch((error) => console.log('error', error))
    },
    reloadPage() {
      window.location.reload()
    }
  },
  beforeMount() {
    this.getItems()
  }
}
</script>

<template>
  <item-edit-modal v-if="modalOpen" @deleted="list => items = list" :data="modalItem"></item-edit-modal>
  <item-new-modal v-if="modalOpen" @new="list => items = list"></item-new-modal>

  <div style="display: flex; justify-content: space-between" class="">
    <button type="button" class="btn btn-info" @click="reloadPage">Neu laden</button>
    <button type="button" class="btn btn-success" data-bs-toggle="modal" data-bs-target="#itemNewModal">
      Neuer Artikel
    </button>
  </div>
  <br>

  <h2>Zu besorgen: {{ getNew.length }} Artikel</h2>
  <div class="item-list-new">
    <table>
      <tr>
        <th>Name</th>
        <th>Mng</th>
        <th></th>
        <th></th>
      </tr>
      <tr v-for="item in getNew">
        <td>
          <img src="@/assets/stores/edeka.jpg" v-if="item.store === 'edeka'" class="button-icon">
          <img src="@/assets/stores/lidl.png" v-if="item.store === 'lidl'" class="button-icon">
          <img src="@/assets/stores/dm.png" v-if="item.store === 'dm'" class="button-icon">
          {{ item.name }} <p class="item-note">{{ item.note }}</p>
        </td>
        <td>{{ item.amount }}</td>
        <td>
          <button type="button" class="btn btn-primary" @click="markAsOld(item.id)">
            <img class="button-icon" alt="Gekauft" src="/src/assets/icons/check-white.svg">
          </button>
        </td>
        <td>
          <button type="button" class="btn btn-warning" data-bs-toggle="modal"
                  data-bs-target="#itemEditModal"
                  @click="openModal(item)">
            <img class="button-icon" alt="Bearbeiten" src="/src/assets/icons/edit-3.svg">
          </button>
        </td>
      </tr>
    </table>

    <br>

    <h2>Zuletzt gekaufte Artikel</h2>
    <div class="item-list-old">
      <table>
        <tr>
          <th>Name</th>
          <th>Mng</th>
          <th></th>
          <th></th>
        </tr>
        <tr v-for="item in getOld">
          <td>
            <img src="@/assets/stores/edeka.jpg" v-if="item.store === 'edeka'" class="button-icon">
            <img src="@/assets/stores/lidl.png" v-if="item.store === 'lidl'" class="button-icon">
            <img src="@/assets/stores/dm.png" v-if="item.store === 'dm'" class="button-icon">
            {{ item.name }} <p class="item-note">{{ item.note }}</p>
          </td>
          <td>{{ item.amount }}</td>
          <td>
            <button type="button" class="btn btn-info" @click="markAsOld(item.id)">
              <img class="button-icon" alt="Erneut kaufen" src="/src/assets/icons/refresh-cw-white.svg">
            </button>
          </td>
          <td>
            <button type="button" class="btn btn-warning" data-bs-toggle="modal"
                    data-bs-target="#itemEditModal"
                    @click="openModal(item)">
              <img class="button-icon" alt="Bearbeiten" src="/src/assets/icons/edit-3.svg">
            </button>
          </td>
        </tr>
      </table>
    </div>
  </div>

  <div v-if="devMode === 'development'">
    <ul>
      <li v-for="item in items">{{ item }}</li>
    </ul>
  </div>
</template>

<style scoped></style>
