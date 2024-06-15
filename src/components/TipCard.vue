<script setup>
import { ref } from 'vue'

const bill = ref(null)
const tip = ref(null)
const customTip = ref(null)
const people = ref(null)
const calculatedTip = ref({
    tipAmount: '0.00',
    total: '0.00'
})
const invalid = ref({
    bill: false,
    tip: false,
    people: false
})

function setTip(amount) {
    if (!amount || amount == NaN) {
        return
    }
    tip.value = parseInt(amount)
    let valid = validate()
    if (valid) {
        calculateTip()
    } else {
        alert("Please enter valid values")
    }
}

function validate() {
    clearError()
    if (!bill.value || bill.value <= 0) {
        invalid.value.bill = "Please enter bill amount"
    }
    if (!tip.value || tip.value <= 0) {
        alert("Please enter a valid tip amount in percentage")
    }
    if (!people.value || people.value <= 0) {
        invalid.value.people = "Can't be zero"
    }
    if (people.value % 1 != 0) {
        invalid.value.people = "Can't be decimal"
    }
    if (!invalid.value.bill && !invalid.value.tip && !invalid.value.people) {
        return true
    } else {
        return false
    }
}
function calculateTip() {
    let billPerPerson = (bill.value / people.value)
    let tipAmount = billPerPerson * tip.value / 100
    let total = billPerPerson + tipAmount
    calculatedTip.value = {
        tipAmount: tipAmount.toFixed(2),
        total: total.toFixed(2)
    }
}

function clearError() {
    invalid.value.bill = false
    invalid.value.tip = false
    invalid.value.people = false
}
function reset() {
    bill.value = null
    tip.value = null
    customTip.value = null
    people.value = null
    calculatedTip.value = {
        tipAmount: '0.00',
        total: '0.00'
    }
    clearError()
}
</script>

<template>
    <div
        class="flex flex-col w-full max-w-lg gap-6 p-6 mx-auto my-4 lg:gap-8 lg:flex-row lg:max-w-4xl rounded-2xl bg-White text-Dark-grayish-cyan">
        <div class="w-full lg:p-2">
            <div class="mb-6">
                <div class="flex items-center justify-between gap-2 mb-2">
                    <label for="bill">Bill</label>
                    <p v-if="invalid.bill" class="text-sm text-orange-600">Input bill amount</p>
                </div>
                <div class="relative">
                    <img src="/images/icon-dollar.svg" alt="$"
                        class="absolute transform -translate-y-1/2 top-1/2 left-3">
                    <input type="number" id="bill" v-model="bill" @change="setTip(tip)"
                        class="w-full py-2 pl-8 pr-4 text-2xl text-right border-2 border-transparent rounded-md text-Very-dark-cyan bg-Very-light-grayish-cyan focus:border-Strong-cyan"
                        :class="{ 'border-orange-600': invalid.bill }" placeholder="0" />
                </div>
            </div>
            <div class="mb-6">
                <div class="mb-2">
                    <label for="tips-select">Select Tip %</label>
                </div>
                <div id="tips-select" class="grid grid-cols-2 gap-4 text-2xl md:grid-cols-3 text-White">
                    <button @click="setTip(5)" :class="{ 'bg-Strong-cyan text-Very-dark-cyan': tip == 5 }"
                        class="w-full p-2 rounded-md bg-Very-dark-cyan active:bg-Light-grayish-cyan active:text-Very-dark-cyan hover:bg-Strong-cyan hover:text-Very-dark-cyan">5%</button>
                    <button @click="setTip(10)" :class="{ 'bg-Strong-cyan text-Very-dark-cyan': tip == 10 }"
                        class="w-full p-2 rounded-md bg-Very-dark-cyan active:bg-Light-grayish-cyan active:text-Very-dark-cyan hover:bg-Strong-cyan hover:text-Very-dark-cyan">10%</button>
                    <button @click="setTip(15)" :class="{ 'bg-Strong-cyan text-Very-dark-cyan': tip == 15 }"
                        class="w-full p-2 rounded-md bg-Very-dark-cyan active:bg-Light-grayish-cyan active:text-Very-dark-cyan hover:bg-Strong-cyan hover:text-Very-dark-cyan">15%</button>
                    <button @click="setTip(25)" :class="{ 'bg-Strong-cyan text-Very-dark-cyan': tip == 25 }"
                        class="w-full p-2 rounded-md bg-Very-dark-cyan active:bg-Light-grayish-cyan active:text-Very-dark-cyan hover:bg-Strong-cyan hover:text-Very-dark-cyan">25%</button>
                    <button @click="setTip(50)" :class="{ 'bg-Strong-cyan text-Very-dark-cyan': tip == 50 }"
                        class="w-full p-2 rounded-md bg-Very-dark-cyan active:bg-Light-grayish-cyan active:text-Very-dark-cyan hover:bg-Strong-cyan hover:text-Very-dark-cyan">50%</button>
                    <input type="number" @keyup="setTip($event.target.value)" id="custom-tip" v-model="customTip"
                        class="w-full p-2 text-center rounded-md text-Very-dark-cyan bg-Very-light-grayish-cyan focus:text-right"
                        placeholder="Custom" />
                </div>
            </div>
            <div>
                <div class="flex items-center justify-between gap-2 mb-2">
                    <label for="people">Number of People</label>
                    <p v-if="invalid.people" class="text-sm text-orange-600">{{ invalid.people }}</p>
                </div>
                <div class="relative">
                    <img src="/images/icon-person.svg" alt="person"
                        class="absolute transform -translate-y-1/2 top-1/2 left-3">
                    <input type="number" id="people" v-model="people" @change="setTip(tip)"
                        class="w-full py-2 pl-8 pr-4 text-2xl text-right border-2 border-transparent rounded-md text-Very-dark-cyan bg-Very-light-grayish-cyan focus:border-Strong-cyan invalid:border-orange-600"
                        :class="{ 'border-orange-600': invalid.people }" placeholder="0" />
                </div>
            </div>
        </div>
        <div class="flex flex-col justify-between w-full gap-8 p-6 rounded-lg lg:p-9 bg-Very-dark-cyan text-White">
            <div class="space-y-6 lg:space-y-8">
                <div class="flex items-center justify-between">
                    <div>
                        <p class="text-sm lg:text-base">Tip amount</p>
                        <p class="text-xs lg:text-sm text-Grayish-cyan">/ person</p>
                    </div>
                    <p class="text-4xl text-right lg:text-5xl text-Strong-cyan"><span>$</span>{{ calculatedTip.tipAmount
                        }}</p>
                </div>
                <div class="flex items-center justify-between">
                    <div>
                        <p class="text-sm lg:text-base">Total</p>
                        <p class="text-xs lg:text-sm text-Grayish-cyan">/ person</p>
                    </div>
                    <p class="text-4xl text-right lg:text-5xl text-Strong-cyan"><span>$</span>{{ calculatedTip.total }}
                    </p>
                </div>
            </div>
            <div>
                <button @click="reset" :disabled="calculatedTip.total == '0.00'" class="w-full p-2 text-2xl tracking-wide rounded-md text-Very-dark-cyan"
                    :class=" calculatedTip.total == '0.00' ? 'bg-Strong-cyan/30 hover:bg-Strong-cyan/30 active:bg-Strong-cyan/30 cursor-not-allowed': 'bg-Strong-cyan hover:bg-Light-grayish-cyan active:bg-Very-light-grayish-cyan cursor-pointer'">
                    RESET
                </button>
            </div>
        </div>
    </div>
</template>

<style scoped>
input[type="number"]::-webkit-inner-spin-button,
input[type="number"]::-webkit-outer-spin-button {
    -webkit-appearance: none;
    margin: 0;
}

input[type="number"] {
    -moz-appearance: textfield;
}

input[type="number"]:hover,
input[type="number"]:focus {
    -moz-appearance: number-input;
    outline: none;
}

input[type="number"]::after {
    content: "$";
    color: black;
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
}
</style>