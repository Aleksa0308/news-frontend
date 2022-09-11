<template>
    <div>
        <h2>
            Naslov: {{ vest.naslov }}
        </h2>

        <h5>Opis:</h5>
        <p>
            {{ vest.tekst }} <br><br>
        </p>
    </div>
</template>
<script>

export default {
    name: "VestItem",
    props: {
        vest: {
            type: Object,
            required: true,
        }
    },
}
</script>
<style>
    
</style>