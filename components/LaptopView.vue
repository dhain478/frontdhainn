<template>
    <div>
        <button class="btn btn-success float-right" @click="onNew">
            New Laptop
        </button>
        <h5>Laptop View</h5>
        <hr>

        <form action="" @submit.prevent="onSave">
            <b-form-group label="Brand">
                <b-form-input v-model="laptop.brand"></b-form-input>
            </b-form-group>
            <b-form-group label="Model">
                <b-form-input v-model="laptop.model"></b-form-input>
            </b-form-group>
            <b-form-group label="Description">
                <b-form-input v-model="laptop.description"></b-form-input>
            </b-form-group>
            <b-form-group label="Price">
                <b-form-input v-model="laptop.price"></b-form-input>
            </b-form-group>
            <b-form-group
              label="Type"
              label-for="Type"
            >
              <b-form-select v-model="laptop.type" :options="types"></b-form-select>
            </b-form-group>
            <b-form-group>
                <button class="btn btn-primary" type="submit">Save Changes</button>
                <button class="btn btn-danger float-right" type="button" @click="onDelete" v-if="laptop.id">Delete Laptop</button>
            </b-form-group>
        </form>
    </div>
</template>

<script>
export default {
    props: {
        laptop: {},
    },
    data() {
      return {
        types: [
          {value: 'gaming laptop', text: 'Gaming Laptop'},
          {value: 'laptop setup gaming', text: 'Laptop Setup Gaming'}
        ]
      }
    },
    methods: {
        async onSave() {
            try {
                if(!this.laptop.id) {
                    await this.$axios.post('/api/laptops', this.laptop)
                }else{
                    await this.$axios.put('/api/laptops/' + this.laptop.id, this.laptop)
                }

                this.$emit('saved')
            } catch (err) {
                alert(err.response.data.message)
            }
        },
        onNew() {
            this.$emit('newLaptop')
        },
        async onDelete() {
            try {
                this.$axios.delete('/api/laptops/' + this.laptop.id)
                this.$emit('deleted')
            } catch (err) {
                alert(err.response.data.message)
            }
        }
    }
}
</script>
