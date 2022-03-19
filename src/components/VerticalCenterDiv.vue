<template>
    <div :style="'position:relative;'+'height:'+height">
        <div :style="'width:'+width+';height:'+height+';overflow:'+overflow+';position:absolute;left:50%;margin-left:'+margin_left">
            <slot></slot>
        </div>
    </div> 
</template>

<script>
export default {
    props:{
        width: "auto",
        height:"auto",
        margin_left:"-50%",
        overflow:"scroll"
    }
}
</script>

