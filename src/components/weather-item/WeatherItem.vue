<template>

<div v-show="isLoaded == true" class="item-wrapper">
    
    <img :src="link" @load="onImageLoad()" alt="Иконка параметра">

    <div class="value-block">
        <span>{{ title }}</span>
        <p>{{ value }}</p>
    </div>

</div>

<div v-show="isLoaded == false" class="item-wrapper">
    
    <b-skeleton type="avatar" class="skeleton-image"/>

    <div class="value-block">
        <b-skeleton type="text" class="skeleton-span"/>
        <b-skeleton type="text" class="skeleton-p"/>
    </div>

</div>


</template>

<script>
export default {
    props: {
        title: {
            type: String,
            required: true
        },
        value: {
            type: String,
            required: true
        },
        link: {
            type: String,
            required: true
        },
        isLoaded: {
            type: Boolean,
            required: true
        },
        setLoaded: {
            type: Function,
            required: true
        }
    },
    methods: {
        onImageLoad () {
            setInterval(() => {
                this.setLoaded(true);
            }, 2500)
        }
    },
}
</script>

<style lang="scss" scoped src="./weather-item.scss"></style>