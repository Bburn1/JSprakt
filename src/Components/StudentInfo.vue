<template>
    <div>
        <table>
             <tr>
                    <th>Photo</th>
                    <th>Name</th>
                    <th>Group</th>
                    <th>Mark</th>
                    <th>PR is Done</th>
                    
                </tr>
        <td> <img v-bind:src="student.photo" width="50px"></td>
        <td>{{student.name}}</td>
        <td>{{student.group}}</td>
         <td>{{student.mark}}</td>
         <td><input type="checkbox" v-bind:checked="student.isDonePr"></td>
         </table>
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
            this.$store.commit('setCount', this.students.length);;
            
        })
},
  computed: {
  studentsCount () {
    return this.$store.getters.getCount
  }

},

}
</script>
