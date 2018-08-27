<template>
  <div id="app">
    <div class='cinemaRoom'>
      <div v-for="(row,rowIndex) in rows" class="row">
        <div
          v-for="(row,seatIndex) in rows"
          class="seat "
          :row="rowIndex+1"
          :seat="seatIndex+1"
          @click="bookIt"
          v-bind:class="{purchased:isPurchased}">
        </div>
      </div>
    </div>
    <div class='result'>
    </div>
    <div class="thanks">Спасибо за заказ!</div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      rows: [10, 10, 10, 10, 10, 10, 10, 10, 10, 10],
      isPurchased: false
    }
  },
  mounted: function() {
    this.fillTheRoom()
  },
  methods: {
    fillTheRoom() {
      // Some places are occupied initially (10 random squares)
      for (let i = 0; i < 10; i++) {
          let theRow = this.getRandomInt(), theSeat = this.getRandomInt();
          this.$el.querySelector('.seat[row="' + theRow + '"][seat="' + theSeat + '"]').classList.add("seat--reserved");
          i === 9 ? this.startToBook() : 0;
      }
    },
    getRandomInt() {
        // just an auxiliary function to obtain an integer (1-10)
        return Math.floor(Math.random() * 10) + 1;
    },
    startToBook() {
        let freeSeats = this.$el.querySelectorAll('.seat:not(.seat--reserved)');
        // for (let i = 0; i < freeSeats.length; i++) {
        //     freeSeats[i].addEventListener('click', function (e) {
        //         // 1st click to buy a ticket ,
        //         // 2nd click to cancel
        //         e.currentTarget.classList.toggle('seat--purchased');
        //         this.showSeatsToBay();
        //     });
        // }
    },
    bookIt() {
      this.isPurchased = !this.isPurchased;
    },
    showSeatsToBay() {
        let result = '<p>Вы выбрали места:</p>', totalPrice = 0,
            purchasedSeats = this.$el.querySelectorAll('.seat.seat--purchased');

        // show the list for purchased places
        for (let i = 0; i < purchasedSeats.length; i++) {
            result += '<div class="ticket">ряд ' +
                purchasedSeats[i].dataset.row + ' место ' +
                purchasedSeats[i].dataset.seat + '</div>';
            totalPrice += 100; // стоимость билета
        }
        result += '<p>Общая стоимость: ' + totalPrice + ' рублей</p>\n' +
            '<button type="button" class="result__buy">Купить</button>\n' +
            '<button type="button" class="result__cancel">Отмена</button>';

        this.result = result;

        // Let's buy
        document.querySelector('.result__buy').addEventListener('click', function (e) {
            for (let i = 0; i < purchasedSeats.length; i++) {
                purchasedSeats[i].classList.add("seat--reserved");
            }
            document.querySelector('.thanks').classList.add("thanks--show");
        });

        // Clear
        document.querySelector('.result__cancel').addEventListener('click', function (e) {
            for (let i = 0; i < purchasedSeats.length; i++) {
                purchasedSeats[i].classList.remove("seat--purchased");
                showSeatsToBay();
            }
        });
      }
    }
  }
</script>

<style lang="scss" scoped>
  #app {
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }

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

  .seat {
      height: 30px;
      width: 30px;
      margin-right: 2px;
      background-color: green;
      display: inline-block;
      cursor: pointer;
  }

  .purchased {
      background-color: yellow;
  }

  .seat--reserved {
      background-color: red;
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
      opacity: 0;
      transition: .75s ease-in-out;
  }

  .thanks--show {
      opacity: 1;
  }
</style>
