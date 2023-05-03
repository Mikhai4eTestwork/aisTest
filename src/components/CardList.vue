<template>
    <b-container class="pt-5">
        <b-row>
            <b-col>
                <div class="border d-flex align-items-center w-100 px-5 py-2 my-5">
                    <div class="app-search-icon flex-shrink-0 d-none d-sm-flex justify-content-center align-items-center overflow-hidden px-5">
                        <b-img-lazy :src="searchIcon"></b-img-lazy>
                    </div>
                    <b-input type="text" placeholder="Введите ЖК / корпус / № квартиры / № паркинга" 
                        class="app-search-input flex-shrink-0 w-100 border-0 bg-transparent"
                        v-model="searchText"
                    ></b-input>
                </div>
            </b-col>
        </b-row>
        <b-row>
            <b-col>
                <div class="app-filter d-flex mb-5">
                    <b-form-checkbox size="lg" v-model="allSelected">все</b-form-checkbox>
                    <b-button variant="secondary" class="app-remove-button border-0 rounded-0 d-flex align-items-center ml-4" @click="removeCard">
                        <span>
                            {{ buttonText }}
                        </span>
                        <div class="app-delete-icon ml-3 d-flex justify-content-center align-items-center">
                            <b-img-lazy :src="deleteIcon"></b-img-lazy>
                        </div>
                    </b-button>
                </div>
            </b-col>
        </b-row>
        <b-row>
            <b-col class="d-flex justify-content-between flex-wrap">
                <CardListItem v-for="(card, index) in cards"
                    v-bind:key="index"
                    v-bind:card="card"
                    v-bind:num="index"
                    v-on:cardFilter="cardFilter"
                    v-on:unsetFilter="unsetFilter"
                >
                </CardListItem>
            </b-col>
        </b-row>
    </b-container>
</template>

<script>
import CardListItem from './CardListItem.vue';

export default {
    data() {
        return {
            searchText: '',
            allSelected: false,
            cards: [
                {
                    price: "7 733 300 руб.",
                    type: "Квартира",
                    icon: "house.svg",
                    status: {
                        text: "Уступка от юр.лица",
                        variant: "danger",
                        code: 1,
                    },
                    checked: false,
                    name: "Чистое небо",
                    address: "корпус 10",
                    date: "10, III кв. 2022 г.",
                    number: "кв 62",
                    rooms: "1 комн.кв.",
                    footage: "234.38 м²",
                    floor: "7 этаж",
                    geo: "Лен. область, Всеволожский район, д. Кудрово, ул. Столичная, д. 5, к. 1",
                    published: "Добавлено 21/11/2020",
                    image: "image2.png",
                    filtered: false,
                    tooltip: 'Другой текст'                    
                },
                {
                    price: "7 733 300 руб.",
                    type: "Квартира",
                    icon: "house.svg",
                    status: {
                        text: "Уступка от физ. лица",
                        variant: "info",
                        code: 2,
                    },
                    checked: false,
                    name: "Зеленый квартал на Пулковских высотах",
                    address: "корпус 11",
                    date: "10, III кв. 2022 г.",
                    number: "кв 73",
                    rooms: "1 комн.кв.",
                    footage: "234.38 м²",
                    floor: "7 этаж",
                    geo: "Комендантский пр., уч. 1 Каменка",
                    published: "Добавлено 21/11/2020",
                    image: "image2.png",
                    filtered: false,     
                    tooltip: 'Еще текст'                                   
                },
                {
                    price: "800 300 руб.",
                    type: "Паркинг",
                    icon: "auto.svg",
                    status: {
                        text: "Забронировано",
                        variant: "secondary",
                        code: 3,
                    },
                    checked: false,
                    name: "Зеленый квартал на Пулковских высотах",
                    address: "корпус 10",
                    date: "10, III кв. 2022 г.",
                    number: "№ 7-10-2 (ПИБ №68)",
                    rooms: "",
                    footage: "234.38 м²",
                    floor: "",
                    geo: "Лен. область, Всеволожский район, д. Кудрово, ул. Столичная, д. 5, к. 1",
                    published: "Добавлено 21/11/2020",
                    image: "image2.png",
                    filtered: false,
                    tooltip: 'Какой-то текст'                    
                },
                {
                    price: "800 300 руб.",
                    type: "Паркинг",
                    icon: "auto.svg",
                    status: {
                        text: "Продано",
                        variant: "dark",
                        code: 4
                    },
                    checked: false,
                    name: "Зеленый квартал на Пулковских высотах",
                    address: "корпус 10",
                    date: "10, III кв. 2022 г.",
                    number: "№ 7-77-8 (ПИБ №69)",
                    rooms: "",
                    footage: "234.38 м²",
                    floor: "",
                    geo: "Комендантский пр., уч. 1 Каменка",
                    published: "Добавлено 21/11/2020",
                    image: "image2.png",
                    filtered: false,
                    tooltip: 'Подземная встроенно-пристроенная'                                        
                },
            ]
        };
    },
    components: { CardListItem },
    computed: {
        deleteIcon: function(){
            return require('../assets/remove.svg')
        },
        searchIcon: function(){
            return require('../assets/search.svg')
        },
        buttonText: function(){
            return (this.allSelected) ? 'Удалить все' : 'Удалить'
        }
    },
    methods: {
        cardFilter: function(code){
            for (let i=0; i < this.cards.length; i++){
                this.cards[i].filtered = (this.cards[i].status.code === code) ? false : true
            }
        },
        unsetFilter: function(){
            for (let i=0; i < this.cards.length; i++){
                this.cards[i].filtered = false
            }            
        },
        removeCard: function(){
            let tempArr = []
            for (let i=0; i<this.cards.length; i++){
                if (!this.cards[i].checked){
                    tempArr.push(this.cards[i])
                }
            }
            this.cards = []
            this.cards = tempArr
        }
    },
    watch: {
        allSelected: function(){
            for (let i=0; i < this.cards.length; i++){
                this.cards[i].checked = this.allSelected
            }
        },
        searchText: function(){
            for (let i=0; i < this.cards.length; i++){
                this.cards[i].filtered = true
            }                        
            const regexp = new RegExp(this.searchText, 'i')
            let success = false
            for (let i=0; i < this.cards.length; i++){
                const a = this.cards[i].name.match(regexp)
                const b = this.cards[i].address.match(regexp)
                const c = this.cards[i].number.match(regexp)
                if (a || b || c){
                    success = true
                    this.cards[i].filtered = false
                }
            }
            if (!success){
                for (let i=0; i < this.cards.length; i++){
                    this.cards[i].filtered = false
                }                 
            }
        }
    }

}
</script>

<style lang="sass" scoped>
.app-delete-icon
    width: 12px
    height: 14px
.app-remove-button
    background-color: #E5E5E5 !important
    color: black !important
.app-search-icon
    width: 14px    
    height: 14px
    padding: 8px
.app-search-input
    box-shadow: none !important
    border: none !important
    outline: none !important    
</style>