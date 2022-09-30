<script>
import axios from "axios";

export default {
    data() {
        return {
            list: [],
            searchTerm: "",
            activeList: false,
        };
    },
    computed: {
        filtered() {
            return this.list.filter((item) => {
                let term = new RegExp("(" + this.searchTerm + ")", "i");
                return item.match(term);
            });
        },
    },
    watch: {
        activeList(val) {
            console.log(val);
            if (val) {
                axios.get("/assets/fonts/FluentSystemIcons-Regular.json").then((res) => {
                    this.list = res.data;
                    this.list = [this.list].map((item, index) => {
                        return Object.keys(item);
                    })[0];
                });
            } else {
                axios.get("/assets/fonts/FluentSystemIcons-Filled.json").then((res) => {
                    this.list = res.data;
                    this.list = [this.list].map((item, index) => {
                        return Object.keys(item);
                    })[0];
                });
            }
        },
    },
    mounted() {
        axios.get("/assets/fonts/FluentSystemIcons-Filled.json").then((res) => {
            this.list = res.data;
            this.list = [this.list].map((item, index) => {
                return Object.keys(item);
            })[0];
        });
    },
    methods: {
        copyBoard: function (event) {
            console.log("ID:", event.target.dataset.id);
            navigator.clipboard.writeText(event.target.innerHTML);
        },
    },
};
</script>
<template>
    <main>
        <section id="icons">
            <div class="container">
                <div class="row mb-3">
                    <div class="col-md-12">
                        <h1 class="text-white display-4 fw-600">Fluent Icon List</h1>
                        <div class="filter-form">
                            <div class="input-group input-group-pd w-fit bg-light gap-1">
                                <button type="button" class="btn text-white fs-5 p-0 ps-2 border-0"><i class="vf-ic_fluent_search_24_filled icon"></i></button>
                                <input type="text" class="form-control-sm bg-light rounded-fill border-0 fs-9 text-white" v-model="searchTerm" placeholder=" Search for icon" />
                            </div>
                            <div class="form-check-group d-flex gap-3">
                                <div class="form-check form-switch">
                                    <label class="form-check-label" for="filled">
                                        Type: <span class="text-primary">{{ activeList ? "Regular" : "Filled" }}</span>
                                    </label>
                                    <input class="form-check-input" type="checkbox" role="switch" name="filled" id="filled" value="filled" v-model="activeList" />
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="row">
                    <div class="col-md-12">
                        <ul class="nav filter-list">
                            <li v-for="(icon, index) in filtered.slice(0, 200)" class="filter-list-item">
                                <span class="icon-format">
                                    <i v-if="!activeList" :class="'vf-' + icon + ' icon'"></i>
                                    <i v-if="activeList" :class="'vr-' + icon + ' icon'"></i>
                                </span>
                                <label class="icon-name user-select-all" @click="copyBoard" :data-id="'iconID' + index">{{ icon }}</label>
                            </li>
                        </ul>
                    </div>
                </div>
            </div>
        </section>
    </main>
</template>
