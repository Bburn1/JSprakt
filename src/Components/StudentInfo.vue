<template>
    <div>
        {{student.name}}
        <img v-bind:src="student.photo">
    </div>
</template>
<script>
import Vue from 'vue'

import VueRouter from 'vue-router'
import axios from 'axios'
import VueAxios from 'vue-axios'

export default {
    props: {
            id: '',
        

},
data: function() {
    return{
        student:{}
    };
},
mounted: function(){
        axios.get("http://46.101.212.195:3000/students/"+this.$route.params.id).then((response)=>{
            console.log(response.data);
            this.student = response.data;
        })
},
}
</script>
