<template lang="html">
    <div class="temp">
        <comment :id=id></comment>
    </div>
</template>

<script>
    import comment from "../subcom/comment.vue";
    export default {
        components : {
            comment
        },
        data(){
            return {
                id : 0
            }
        },
        created(){
            this.id = this.$route.params.id;
        }
    }
</script>

<style lang="css" scoped>

</style>
