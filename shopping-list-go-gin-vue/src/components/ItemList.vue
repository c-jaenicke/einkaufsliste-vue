<script setup>
import './ItemEdit.vue'
import ItemEditModal from "./ItemEdit.vue";
import NewItemModal from "./NewItemModal.vue";
import ItemNewModal from "./NewItemModal.vue";
</script>

<script>
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
            // this.modalOpen = true
        },
        getItems() {
            this.items = []
            const endpoint = 'http://localhost:8080/items/all'
            console.log(endpoint)
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
            const endpoint = 'http://localhost:8080/item/' + id + '/switch'
            console.log(endpoint)
            const requestOptions = {
                mode: 'no-cors',
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
        reloadPage(){
            window.location.reload()
        }
    },
    beforeMount() {
        this.getItems()
    }
}
</script>

<template>
    <item-edit-modal v-if="modalOpen" @close="getItems()" :data="modalItem"></item-edit-modal>
    <item-new-modal v-if="modalOpen" @close="getItems()"></item-new-modal>

    <div style="display: flex; justify-content: space-between" class="">
        <button type="button" class="btn btn-info" @click="reloadPage">Neu laden</button>
        <button type="button" class="btn btn-success" data-bs-toggle="modal" data-bs-target="#itemNewModal">Neuer Artikel</button>
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
                <td>{{ item.name }} {{ item.id }}</td>
                <td>{{ item.note }}</td>
                <td>{{ item.amount }}</td>
                <td>
                    <a href="#" @click="markAsOld(item.id)" class="btn btn-primary">
                        <img alt="Gekauft" src="/src/assets/icons/check-square-white.svg">
                    </a>
                </td>
                <td>
                    <button type="button" class="btn btn-primary" data-bs-toggle="modal"
                            data-bs-target="#itemEditModal"
                            @click="openModal(item)">
                        <img alt="Bearbeiten" src="/src/assets/icons/edit-3-white.svg">
                    </button>
                </td>
            </tr>
        </table>

        <br>

        <h2>Zuletzt gekauft:</h2>
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
                    <td>{{ item.name }} {{ item.id }}</td>
                    <td>{{ item.note }}</td>
                    <td>{{ item.amount }}</td>
                    <td>
                        <a href="#" @click="markAsOld(item.id)" class="btn btn-primary">
                            <img alt="Gekauft" src="/src/assets/icons/check-square-white.svg">
                        </a>
                    </td>
                    <td>
                        <button type="button" class="btn btn-primary" data-bs-toggle="modal"
                                data-bs-target="#itemEditModal"
                                @click="openModal(item)">
                            <img alt="Bearbeiten" src="/src/assets/icons/edit-3-white.svg">
                        </button>
                    </td>
                </tr>
            </table>
        </div>
    </div>
</template>

<style scoped></style>
