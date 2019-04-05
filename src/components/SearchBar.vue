<template>
    <div class="container-fluid">
        <div class="row">
            <div class="col-md-6 offset-md-3">
                <div class="input-group">
                    <input type="text" v-model="address" class="form-control">
                    <div class="input-group-append">
                        <button @click="clear" class="btn btn-secondary" type="button">{{clearButtonTitle}}</button>
                        <button @click="search" class="btn btn-primary" type="button">{{searchButtonTitle}}</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
  name: "SearchBar",
  props: {
    searchButtonTitle: {
        default: 'Search',
        type: String
    },
    clearButtonTitle: {
        default: 'Clear',
        type: String
    },
  },
  data: () => {
    return {
        address: null,
    }
  },
  methods: {
    search() {

        if (! this.address) {
            this.$emit('error', 'Please, enter a valid address.');
            return false;
        }

        this.$emit('search', this.address);
    },
    clear() {
        this.address = null;
        this.$emit('clear');
    }
  }
};
</script>

<style scoped>
.container-fluid {
    padding-top: 25px;
    padding-bottom: 10px;
}
</style>
