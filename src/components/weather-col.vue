<template>
    <div class="weather-col">
        <div class="text-center">
            <p class="column-day">{{day}}</p>
        </div>
        <div class="text-center temp">
            <span>{{temperature}}&deg;C</span>
        </div>
        <div class="condition-temp">
            <p class="condition">{{description}}</p>
            <p class="high">{{high}}&deg;C</p>
            <p class="low">{{low}}&deg;C</p>
        </div>
        <div class="card-bottom px-2 py-4 row">
            <div class="col text-center">
                <p>{{clouds}}%</p>
                <span>Clouds</span>
            </div>
            <div class="col text-center">
                <p>{{wind}} m/s</p>
                <span>Speed of Wind</span>
            </div>
        </div>
    </div>
</template>

<script>

export default ({
    name: "weather-col",
    props: [
        'day',
        'temperature',
        'description',
        'high',
        'low',
        'clouds',
        'wind'
    ],
})
</script>
