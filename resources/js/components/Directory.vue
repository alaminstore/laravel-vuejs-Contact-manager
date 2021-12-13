<template>
    <div>
        <h2 class="text-center">{{ projectName }}</h2>
        <div class="w-50 m-auto">
            <div class="form-group">
                <label for="name">Name:</label>
                <input type="text"
                       id="name"
                       placeholder="Name here..."
                       class="form-control"
                       v-model="item.name"
                >
            </div>
            <div class="form-group">
                <label for="phone">Phone Number:</label>
                <input type="number"
                       id="phone"
                       placeholder="Enter Phone Number..."
                       class="form-control"
                       v-model="item.phone"
                >
            </div>
            <div class="d-grid gap-2 col-12 mx-auto mt-2">
                <button @click="save" class="btn button btn-dark btn-block btn-sm">
                    {{ isEditing ? "Update Data" : "Save Data" }}
                </button>
            </div>
            <div class="col-md-12 mt-3" v-if="lists.length > 0">
                <h3 class="text-center">User Name & Number</h3>
                <ul class="list-group">
                    <li class="list-group-item" v-for="list in lists" :key="list.id">
                        {{ list.name }} - {{ list.phone }}
                        <span class="float-end">
                        <button class="btn btn-sm btn-warning mr-2"
                                @click="editTel(list)"
                        >Edit
                        </button>
                        <button class="btn btn-sm btn-danger"
                                @click="deleteTel(list.id)"
                        >Delete
                        </button>
                    </span>
                    </li>
                </ul>
            </div>


        </div>
    </div>
</template>

<script>
export default {
    name: "Directory",
    data() {
        return {
            projectName: 'Crud Operation by Vue',
            lists: [],
            item: {
                name: '',
                phone: ''
            },
            temp_id: null,
            isEditing: false
        }
    },
    mounted() {
        this.fetchAll();
        console.log('fk id',this.temp_id);
    },
    methods: {
        fetchAll() {
            axios.get('api/tel')
                .then(res => this.lists = res.data)
        },
        save() {
            let method = axios.post;
            let url = '/api/tel';

            if (this.isEditing) {
                method = axios.put;
                url = `/api/tel/41`

            }
            try {
                method(url, this.item)
                    .then(res => {
                        this.item = {
                            name: "",
                            phone: ""
                        },
                            this.fetchAll();
                        this.temp_id = null;
                        this.isEditing = false;
                    });
            } catch (e) {
                console.log(e);
            }
        },
        editTel(tel) {
            this.item = {
                name: tel.name,
                phone: tel.phone
            },
                this.isEditing = true;
            this.temp_id = tel.id;
        },
        deleteTel(id) {
            try {
                axios.delete(`/api/tel/${id}`)
                    .then(res => this.fetchAll())
            } catch (e) {
                console.log(e)
            }
        }
    }
}
</script>

<style scoped>

</style>
