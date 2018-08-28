<template>
    <div
        class="seat"
        @click="toggleBook"
        :class="{purchased:isPurchased}">
        &nbsp;
    </div>
</template>

<script>
    export default {
        name: 'seat',
        data() {
            return {
                isPurchased: false
            }
        },
        methods: {
            toggleBook() {
                // 1st click to buy a ticket ,
                // 2nd click to cancel
                !this.$el.classList.contains('seat--reserved') ? this.isPurchased = !this.isPurchased : 0;
                this.$emit('showSeatsToBay');
            }
        }
    }
</script>

<style lang="scss" scoped>
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
</style>
