<template>
    <div class="calendar-box">
    <div class="lang-button" @click="chLang">
        {{lang}}
    </div>
        <div class="calendar-header">
            <button class="pre-button" @click="pre_month">
                <i class="fas fa-caret-down"></i>
            </button>  
                <p class="choise-data">
                    {{getMonth}} {{getYear}}
                </p>
            <button class="next-button" @click="next_month">
                <i class="fas fa-caret-down"></i>
                +
            </button>
        </div>

        <ul class="week-days en" v-if="lang=='en'">
            <li class="day-name" v-for="dayName in dayNamesEN" :key="dayName">{{dayName}}</li>
        </ul>
        <ul class="week-days ru" v-else>
            <li class="day-name" v-for="dayName in dayNamesRU" :key="dayName">{{dayName}}</li>
        </ul>

        <ul class="day-list">
            <li v-for="day_number in getMonthDays" 
                :key="day_number"
                @click="choiseDay(day_number)"
                :class="getClasses(day_number)">
                {{day_number}}
            </li>
        </ul>
    </div>
</template>

<script>
import arrowLeft from './arrowLeft'
export default {
    data() {
        return {
            date: {
                day:1,
                month: 1,
                year: 1
            },
            lang: 'en',
            pre_state: {},
            dayNamesEN: ['Sun', "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"],
            dayNamesRU: ['Вс', 'Пн', 'Вт', 'Ср', 'Чт', 'Пт', 'Сб'],
            mounthesEN: ['Jan', 'Feb', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'],
            mounthesRU: ['Январь', 'Февраль', 'Март', 'Апрель', 'Май', 'Июнь', 'Июль', 'Август', 'Сентябрь', 'Октябрь', 'Ноябрь', 'Декабрь']
        }
    },
    created() {
        if(this.day&&this.month&&this.day){
            this.date.year = this.year
            this.date.month = this.month
            this.date.day = this.day
            this.pre_state.year = this.year
            this.pre_state.month = this.month
            this.pre_state.day = this.day
        } else {
            this.date.year = new Date().getFullYear()
            this.date.month = new Date().getMonth()+1
            this.date.day = new Date().getDate()

            this.pre_state.year = new Date().getFullYear()
            this.pre_state.month = new Date().getMonth()+1
            this.pre_state.day = new Date().getDate()
        }
    },
    computed: {
        getMonthDays(){ 
            return 28 + ((this.date.month + Math.floor(this.date.month / 8)) % 2) + 2 % this.date.month + Math.floor((1 + (1 - (this.date.year% 4 + 2) % (this.date.year % 4 + 1)) * ((this.date.year % 100 + 2) % (this.date.year % 100 + 1)) + (1 - (this.date.year % 400 + 2) % (this.date.year % 400 + 1))) /this.date.month) + Math.floor(1/this.date.month) - Math.floor(((1 - (this.date.year % 4 + 2) % (this.date.year % 4 + 1)) * ((this.date.year % 100 + 2) % (this.date.year % 100 + 1)) + (1 - (this.date.year % 400 + 2) % (this.date.year % 400 + 1)))/this.date.month); 
        },
        getYear(){
            return this.date.year
        },
        getMonth(){
            if(this.lang == 'en'){
                return this.mounthesEN[this.date.month-1]
            } else {
               return this.mounthesRU[this.date.month-1]
            } 
        },
    },
    methods: {
        chLang(){
            if(this.lang == 'en'){
                this.lang = 'ru'
            } else {
                this.lang = 'en'
            }
        },
        choiseDay(day_n){
            this.date.day = day_n
            this.pre_state.year = this.date.year
            this.pre_state.month = this.date.month
            this.pre_state.day = this.date.day
        },
        pre_month(){
            console.log(this.date.month, this.date.year)
            if(this.date.month<2){
                this.date.month=12
                this.date.year--
            } else {
                this.date.month--
            }
        },
        next_month(){
            console.log(this.date.month, this.date.year)
            if(this.date.month>10){
                this.date.month=1
                this.date.year++
            } else {
                this.date.month++
            }
        },
        getClasses(day_number){
            if(day_number==new Date().getDate()==this.date.day&& this.date.month==new Date().getMonth()+1 &&this.date.year==new Date().getFullYear()){
                return 'day today choised'
            }
            else if(day_number==this.pre_state.day&&this.date.month==this.pre_state.month&&this.date.year==this.pre_state.year){
                return 'day choised'
            } else if(day_number==new Date().getDate()&&new Date().getMonth()==this.date.month-1&&new Date().getFullYear()==this.date.year){
                return 'day today'
            }
        return 'day'
        }
    }
}
</script>

<style scoped>
    .lang-button{
        position: absolute;
        right: 30px;
        top: 30px;
        border: 1px solid black;
        padding: 3px;
    }
    .calendar-box{
        width: 200px;
        height: 200px;
        border: 1px solid #ccc;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: start;
        padding: 3px;
    }
    .calendar-header{
        display: flex;
        width: 100%;
        height: 10%;
        justify-content: space-between;
        align-items: center;
    }
    .week-days{
        width: 100%;
        padding: 0;
        display: grid;
        grid-template-rows: 1fr;
        grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr 1fr;
        list-style: none;
        font-size: 10px;
    }
    .day-list{
        margin-top: -2px;
        padding: 0;
        list-style: none;
        width: 100%;
        display: grid;
        grid-template-rows: 1fr 1fr 1fr 1fr 1fr;
        grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr 1fr;
        grid-gap: 2px;
    }
    .day{
        cursor: pointer;
    }
    .day:hover{
        background: rgba(0, 163, 185, 0.384);
    }
    .today{
        border: 2px solid black;
    }
    .choised{
        background: #ccc;
    }
</style>