<template>
    <div>
        <h2 class="text-blue-600">Wpisy na bloga:</h2>
        <div class="w-100 flex flex-row-reverse">
            <button @click="pobierzWpisy" class="bg-blue-600 rounded-md text-white p-4 hover:bg-blue-800">Odśwież wpisy</button>
        </div>
        <div class="grid mx-6 gap-4 my-4">
            <div v-for="(wpis, index) in wpisy" class="drop-shadow-md bg-stone-300 p-4">
                <p>id: {{ index }}</p>
                <p>{{ wpis }}</p>
                <br>
                <button @click="deleteWpis(index)" class="bg-blue-600 rounded-md text-white p-4 hover:bg-blue-800">Usuń wpis</button>
                <br>
                <br>
                <input v-model="edytowanyWpis" type="text" class="border-2 border-blue-600 p-4">
                <button @click="edytujWpis(index)" class="bg-blue-600 rounded-md text-white p-4 hover:bg-blue-800 gap-4 mx-10">Edytuj wpis</button>
            </div>
        </div>
    <div class="flex justify-center flex-col">
        <input v-model="nowyBlog" type="text" class="border-2 border-blue-600 p-4">
        <br>
        <button @click="dodajWpisy" class="bg-blue-600 rounded-md text-white p-4 hover:bg-blue-800">Dodaj wpis</button>
    </div>
    </div>
</template>

<script>
import { dzien2_backend } from 'declarations/dzien2_backend/index';

export default {
    data(){
        return {
            wpisy: [],
            nowyBlog: "",
            edytowanyWpis: ""
        }
    },
    methods: {
        async dodajWpisy() {
           await dzien2_backend.dodaj_wpis(this.nowyBlog);
           await this.pobierzWpisy();
        },
        async deleteWpis(index) {
           await dzien2_backend.usun_wpis(index);
           await this.pobierzWpisy();
        },
        async edytujWpis(index, text) {
           await dzien2_backend.edytuj_wpis(index, this.edytowanyWpis);
           await this.pobierzWpisy();
        },
        async pobierzWpisy() {
           this.wpisy = await dzien2_backend.odczytaj_wpisy();
        }
    },
    async mounted(){
        this.pobierzWpisy()
    }

}
</script>