<template>
    <div>
        {{students.name}}
        <img v-bind:src="student.photo">
    </div>
</template>
<script>
import Vue from 'vue'

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
        axios.get("http://46.101.212.195:3000/students").then((response)=>{
            console.log(response.data);
            this.students = response.data;
        })
},
}
</script>
