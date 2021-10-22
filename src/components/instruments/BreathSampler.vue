<template>
    <div>
        <h1>breath</h1>
    </div>
</template>

<script>
export default {
    props: ['breath'],
    data() {
        return {
            breathIntensity: 0
        }
    },
    watch: {
        breath(newValue) {
            console.log(newValue);
        }
    }
}
</script>