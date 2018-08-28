<template>
    <div id="app">
        <div class='cinemaRoom'>
            <div v-for="(row,rowIndex) in rows" class="row">
                <seat
                    v-for="(seat,seatIndex) in row"
                    :key="seatIndex"
                    :row="rowIndex+1"
                    :seat="seatIndex+1"
                    @showSeatsToBay="showSeatsToBay">
                </seat>
            </div>
        </div>
        <div class='result' v-if="seats.length">
            <p>Вы выбрали места:</p>
            <div class="ticket" v-for="ticket in seats">
                <div class="ticket">ряд {{ticket.row}} место {{ticket.seat}}</div>
            </div>
            <p>Общая стоимость: {{100*seats.length}} рублей</p>
            <button type="button" @click="bayTheTickets">Купить</button>
            <button type="button" @click="clearList">Отмена</button>
        </div>
        <div class='result' v-else>Выберете место</div>
        <transition name="fade">
            <div class="thanks" v-if="thanks">Спасибо за заказ!</div>
        </transition>
    </div>
</template>

<script>
    import seat from './components/Seat.vue'

    export default {
        components: {
            seat: seat
        },
        name: 'app',
        data() {
            return {
                rows: [10, 10, 10, 10, 10, 10, 10, 10, 10, 10],
                seats: {},
                result: '',
                thanks: false
            }
        },
        mounted: function () {
            // Some places are occupied initially (10 random squares)
            for (let i = 0; i < 10; i++) {
                let theRow = this.getRandomInt(), theSeat = this.getRandomInt();
                this.$el.querySelector('.seat[row="' + theRow + '"][seat="' + theSeat + '"]').classList.add("seat--reserved");
            }
        },
        methods: {
            getRandomInt() {
                // just an auxiliary function to obtain an integer (1-10)
                return Math.floor(Math.random() * 10) + 1;
            },
            showSeatsToBay() {
                // Show the seats you want to buy
                this.seats = this.$children.filter(element => element.isPurchased && !element.$el.classList.contains('seat--reserved'));
                this.seats.forEach(function(element) {
                    element.row = element.$attrs.row;
                    element.seat = element.$attrs.seat;
                });
            },
            bayTheTickets(){
                // Let's buy
                this.thanks=true;
                this.$el.querySelectorAll('.seat.purchased').forEach(function(element) {
                    element.classList.add("seat--reserved");
                });
            },
            clearList(){
                // Clear
                this.thanks=false;
                this.seats.forEach(function(element) {
                    element.$el.classList.remove("purchased");
                });
                this.seats.splice(0, this.seats.length);
                this.$children.filter(element => element.isPurchased).forEach(function(element) {
                    !element.$el.classList.contains("seat--reserved") ? element.isPurchased = false : 0;
                });
            }
        }

    }
</script>

<style lang="scss" scoped>
    * {
        margin: 0;
    }

    body {
        padding: 20px;
    }

    .cinemaRoom {
        text-align: center;
        display: inline-block;
        vertical-align: top;
    }

    .row {
        margin-bottom: 2px;
    }

    .result {
        font-size: 18px;
        display: inline-block;
        width: 290px;
        margin-right: 5px;
    }

    .thanks {
        font-size: 90px;
        color: green;
    }
    .fade-enter-active, .fade-leave-active {
        transition: opacity .5s;
    }
    .fade-enter, .fade-leave-to {
        opacity: 0;
    }
</style>
