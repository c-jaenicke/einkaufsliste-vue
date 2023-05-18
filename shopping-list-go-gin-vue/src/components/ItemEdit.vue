<script>
export default {
    name: 'ItemEditModal',
    data() {
        return {
            amount: 0,
            name: '',
            description: '',
        }
    },
    props: {
        data: {
            type: Object
        }
    },
    methods: {
        subtractAmount() {
            if (this.data.amount > 0) {
                return this.data.amount - 1
            } else {
                return this.data.amount
            }
        },
        addAmount() {
            if (this.data.amount < 100) {
                return this.data.amount + 1
            } else {
                return this.data.amount
            }
        },
        updateItem() {
            const endpoint = 'http://localhost:8080/item/' + this.data.id + '/update'
            console.log(endpoint)
            const requestOptions = {
                method: 'PUT',
                headers: {"Content-Type": "application/json"},
                body: JSON.stringify(this.data)
            }
            fetch(endpoint, requestOptions)
                .catch((error) => console.log('error', error))
        },
        deleteItem() {
            const endpoint = 'http://localhost:8080/item/' + this.data.id + '/delete'
            console.log(endpoint)
            const requestOptions = {
                method: 'DELETE',
            }
            fetch(endpoint, requestOptions)
                .catch((error) => console.log('error', error))
        }
    }
}
</script>

<template>
    <div class="modal fade" id="itemEditModal" tabindex="-1" aria-labelledby="itemEditModalLabel" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h1 class="modal-title fs-5" id="itemEditModalLabel">Eintrag bearbeiten</h1>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"
                            @click="$emit('close')"></button>
                </div>
                <div class="modal-body">
                    <label for="item-name" class="form-label">Name</label>
                    <div class="input-group mb-3">
                        <input class="form-control" type="text" name="name" id="item-name" v-model="data.name"
                               maxlength="20">
                    </div>

                    <label for="item-note" class="form-label">Beschreibung</label>
                    <div class="input-group mb-3">
                        <input class="form-control" type="text" name="note" id="item-note" v-model="data.note"
                               maxlength="30">
                    </div>

                    <label for="item-amount" class="form-label">Menge</label>
                    <div class="input-group mb-3">
                        <button class="btn btn-outline-secondary" type="button" id="button-subtract-amount"
                                @click="data.amount = subtractAmount()">-
                        </button>
                        <input class="form-control" type="number" name="amount" id="item-amount" v-model="data.amount"
                               min="0" max="100">
                        <button class="btn btn-outline-secondary" type="button" id="button-add-amount"
                                @click="data.amount = addAmount()">+
                        </button>
                    </div>

                </div>
                <div class="modal-footer justify-content-between">
                    <button type="button" class="btn btn-danger" @click="deleteItem(); $emit('close')" data-bs-dismiss="modal">Löschen
                    </button>
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal" @click="$emit('close')">
                        Abbrechen
                    </button>
                    <button type="button" class="btn btn-primary" @click="updateItem()" data-bs-dismiss="modal">
                        Speichern
                    </button>
                </div>
            </div>
        </div>
    </div>

</template>

<style scoped>

</style>