<template>
    <div id="foot">
        <p :style="{color: color}">Copyright ©Badreddine Asmi</p>
    </div>
</template>

<script>
export default {
    name: "foot",
    props: ['color']
}
</script>
<style scoped>
#foot {
    height: 5vh;
    display: flex;
    justify-content: center;
    align-items: center;
}
</style>