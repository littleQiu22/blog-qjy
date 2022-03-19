<template>
    <!-- <div style="position:relative;height:250px">
        <div :style="'width:'+width+';position:absolute'+';left:50%'+';margin-left:'+margin_left+';overflow:'+overflow">
            <slot name="body"></slot>
        </div>
    </div> -->
    <div :style="'position:relative;'+'height:'+outer_height">
        <div :style="'width:'+width+';height:'+inner_height+';overflow:'+overflow+';position:absolute;left:50%;margin-left:'+margin_left">
            <slot name="body"></slot>
        </div>
    </div> 
</template>

<script>
export default {
    props:{
        width: "auto",
        outer_height:"auto",
        inner_height:"auto",
        margin_left:"-50%",
        overflow:"scroll"
    }
}
</script>

