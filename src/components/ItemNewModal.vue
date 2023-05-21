<script>
const url = import.meta.env.VITE_API_BASE_URL
export default {
    name: 'item-new-modal',
    data() {
        return {
            items: [],
            amount: 0,
            name: '',
            note: '',
        }
    },
    methods: {
        subtractAmount() {
            if (this.amount > 0) {
                return this.amount - 1
            } else {
                return this.amount
            }
        },
        addAmount() {
            if (this.amount < 100) {
                return this.amount + 1
            } else {
                return this.amount
            }
        },
        createItem() {
            this.items = []
            const endpoint = 'http://'+ url + '/item/new'
            const requestOptions = {
                method: 'POST',
                headers: {"Content-Type": "application/json"},
                body: JSON.stringify({
                    "name": this.name,
                    "note": this.note,
                    "amount": this.amount,
                    "status": "new",
                    "cat_id": 0
                })
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
        resetInput(){
          this.name = ''
          this.note =''
          this.amount = 0
        },
    }
}
</script>

<template>
    <div class="modal fade" id="itemNewModal" tabindex="-1" aria-labelledby="itemNewModalLabel" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h1 class="modal-title fs-5" id="itemNewModalLabel">Neuen Artikel anlegen</h1>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"
                            @click="$emit('close')"></button>
                </div>
                <div class="modal-body">
                    <label for="item-name" class="form-label">Name</label>
                    <div class="input-group mb-3">
                        <input class="form-control" type="text" name="name" id="item-name" v-model="name"
                               maxlength="20">
                    </div>

                    <label for="item-note" class="form-label">Beschreibung</label>
                    <div class="input-group mb-3">
                        <input class="form-control" type="text" name="note" id="item-note" v-model="note"
                               maxlength="30">
                    </div>

                    <label for="item-amount" class="form-label">Menge</label>
                    <div class="input-group mb-3">
                        <button class="btn btn-outline-secondary" type="button" id="button-subtract-amount"
                                @click="amount = subtractAmount()">-
                        </button>
                        <input class="form-control" type="number" name="amount" id="item-amount" v-model="amount"
                               min="0" max="100">
                        <button class="btn btn-outline-secondary" type="button" id="button-add-amount"
                                @click="amount = addAmount()">+
                        </button>
                    </div>

                </div>
                <div class="modal-footer justify-content-between">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal" @click="$emit('close'); resetInput()">
                        Abbrechen
                    </button>
                    <button type="button" class="btn btn-primary" @click="createItem(); $emit('new', this.items ); resetInput()"
                            data-bs-dismiss="modal">
                        Speichern
                    </button>
                </div>
            </div>
        </div>
    </div>

</template>

<style scoped>

</style>