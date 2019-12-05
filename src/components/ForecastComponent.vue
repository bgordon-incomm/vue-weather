<template>
    <div>
        <form @submit.prevent="parseInput()" class="enter-zip">
            <label>Enter zip/City, State</label>
            <input type="text" v-model="zip" class="zip-input"/>
            <button v-on:click="printJSON">Get forecasts</button>
        </form>
        <table>
            <thead>
                <tr>
                    <th>Day</th>
                    <th>Temperature</th>
                    <th>Forecast</th>
                </tr>
            </thead>
            <tbody>
                <tr v-bind:key="forecast.id" v-for="forecast in forecasts">
                    <td>{{forecast.name}}</td>
                    <td>{{forecast.temperature}}{{forecast.temperatureUnit}}</td>
                    <td>{{forecast.detailedForecast}}</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
import json from '../assets/forecast.json'
import Zipcodes from 'zipcodes'

export default {

    name: 'forecast-component',

    data() {
        return {
            forecasts: [],
            myJSON: json
        }
    },

    methods: {

        parseInput() {
            const input = this.zip
            if (isNaN(input)) {
                let cityAndState = input.split(", ")
                let area = Zipcodes.lookupByName(cityAndState[0], cityAndState[1])
                let lat = area[0].latitude
                let long = area[0].longitude
                this.getForecast(lat, long)
            } else {
                let area = Zipcodes.lookup(input)
                let lat = area.latitude
                let long = area.longitude
                this.getForecast(lat, long)
            }
        },

        printJSON() {
            this.forecasts = json
            // console.log(json.properties.periods)
        },

        async getForecast(latitude, longitude) {
            this.$emit('getForecast', this.zip)
            try {
                // let point = Zipcodes.lookup(this.zip)
                // this.$emit('Zipcode object', point)
                fetch(`https://api.weather.gov/points/${latitude},${longitude}`)
                .then(response => response.json())
                .then(body => {
                    console.log(body)
                    const serviceCall = body.properties.forecast
                    fetch(serviceCall)
                    .then(response => response.json())
                    .then(body => {
                        this.forecasts = body.properties.periods
                    })
                })
            } catch(error) {
                console.error(error)
            }
        },
    }
}
</script>

<style>
    .zip-input {
        width: 30%;
        margin: auto;
    }
</style>
