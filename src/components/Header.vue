<template>
    <div>
        <b-nav id="nav" tabs>
            <b-nav-item id="game-title" disabled><b>TRIVIA <br> Ten Quiz Game</b></b-nav-item>
            <b-nav-item disabled>Counter: {{ numCorrect }}/{{ numTotal }}</b-nav-item>
            <b-nav-item id="select-container">
                <b-form-select id="select" v-model="selected" :options="options" size="sm"></b-form-select>
            </b-nav-item>

        </b-nav>
    </div>
</template>

<script>

export default {
    props: [
        'numCorrect',
        'numTotal'
    ],

    data() {
        return {
            selected: null,
            options: [
                { value: null, text: 'Please select an option' },
                { value: 'a', text: 'This is First option' },
                { value: 'b', text: 'Selected Option' },
                { value: { C: '3PO' }, text: 'This is an option with object value' },
                { value: 'd', text: 'This one is disabled', disabled: true }
            ]
        }
    }
}

</script>

<style>

 #select-container {
    position: absolute;
    top: 4.6rem;
    right: 1rem;
    display: none;
 }

 #select {
     border: 1px solid lightgray;
 }

</style>