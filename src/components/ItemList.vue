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
            const endpoint = 'http://' + url + '/items/all'
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
            const endpoint = 'http://' + url + '/item/' + id + '/switch'
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
                <th>Notiz</th>
                <th>Menge</th>
                <th></th>
                <th></th>
            </tr>
            <tr v-for="item in getNew">
                <td v-if="devMode === 'development'">{{ item.name }} {{ item.id }}</td>
                <td v-else>{{ item.name }}</td>
                <td>{{ item.note }}</td>
                <td>{{ item.amount }}</td>
                <td>
                    <button type="button" class="btn btn-primary" @click="markAsOld(item.id)">
                        <img alt="Gekauft" src="/src/assets/icons/check-white.svg">
                    </button>
                </td>
                <td>
                    <button type="button" class="btn btn-warning" data-bs-toggle="modal"
                            data-bs-target="#itemEditModal"
                            @click="openModal(item)">
                        <img alt="Bearbeiten" src="/src/assets/icons/edit-3.svg">
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
                    <th>Notiz</th>
                    <th>Menge</th>
                    <th></th>
                    <th></th>
                </tr>
                <tr v-for="item in getOld">
                    <td v-if="devMode === 'development'">{{ item.name }} {{ item.id }}</td>
                    <td v-else>{{ item.name }}</td>
                    <td>{{ item.note }}</td>
                    <td>{{ item.amount }}</td>
                    <td>
                        <button type="button" class="btn btn-info" @click="markAsOld(item.id)">
                            <img alt="Erneut kaufen" src="/src/assets/icons/check.svg">
                        </button>
                    </td>
                    <td>
                        <button type="button" class="btn btn-warning" data-bs-toggle="modal"
                                data-bs-target="#itemEditModal"
                                @click="openModal(item)">
                            <img alt="Bearbeiten" src="/src/assets/icons/edit-3.svg">
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
