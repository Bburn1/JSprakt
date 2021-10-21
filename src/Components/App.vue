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

                <tr v-for="stud in students" v-bind:key="stud._id">
                    <td><img v-bind:src="stud.photo" width="50px"></td>

                    <td v-bind:style="stud.name.indexOf(search)>-1 && search.length >0 ? 'background-color:#CA2C2C' : 'background-color:#fff'">{{stud.name}}<input v-model="name1" v-on:click="updateStudent()" v-bind:style="showInput ? 'display:inline' : 'display:none'"></td>                           
                    <td>{{stud.group}} <input v-model="group1" v-on:click="updateStudent()" v-bind:style="showInput ? 'display:inline' : 'display:none'"></td>
                    <td>{{stud.mark}}<input v-model="mark1" v-on:click="updateStudent()" v-bind:style="showInput ? 'display:inline' : 'display:none'"></td>
                    <td><input type="checkbox" v-bind:checked="stud.isDonePr"> <input type="checkbox" v-model="isDonePr1" v-on:click="updateStudent()" v-bind:style="showInput ? 'display:inline' : 'display:none'"></td>
                    <td> <a href="#" v-on:click="deleteStudent(stud._id)">Delete</a></td>
                    <td><button v-on:click="getData(stud._id,stud.mark,stud.isDonePr,stud.name, stud.group)" ><img src="1.png" width="20px"></button>
                    <button v-on:click="updateStudent()" v-bind:style="showInput ? 'display:inline' : 'display:none'">Edit</button></td>
                </tr>
            </table>



            <br> <h2>Add Student : </h2>
            <br> Name  <input v-model="name">
            <br> Group <input v-model="group">
            <br> Mark <input type="number"  v-model="mark">
            <br> PR <input type="checkbox" v-model="isDonePr">
            <br> <button v-on:click="addStudent">Add</button>



            <br>Enter second name: <input v-model="search">
			<hr>


            <h3>Currency converter</h3>
            <br>Enter amount: <input type="number" value="100" v-model="start_value">
            <br>Convert from <input list="currency"  v-model="start_ccy">
            <datalist id="currency">
             
            <option value="CAD"> </option>
            <option value="CHF"></option>
            <option value="CZK"></option>
            <option value="GBP"></option>
            <option value="ILS"></option>
            <option value="JPY"></option>
            <option value="NOK"></option>
            <option value="PLZ"></option>
            <option value="SEK"></option>
            <option value="BTC"></option>
                                            
                                            
            </datalist>
             <input list="currency" v-model="end_ccy">
            <button v-on:click="convert">Convert to</button>
            <br>{{result}}
   </div>
</template>

<script>
    
import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'


    export default {
    data: function() {
        return {
        students: [],
        currency:[],
        mark1: "",
        name1:"",
        group1:"",
        isDonePr1:false,
        showInput:false,
        name:"",
        group:"",
        studId:"",
        isDonePr:false,
        search:"",
        mark: 0,
        start_ccy:"",
        end_ccy:"",
        sell:0,
        buy:0,
        start_value:0,
        end_value:0,
        result:"",
    }},
    mounted: function(){
        axios.get("http://46.101.212.195:3000/students").then((response)=>{
            console.log(response.data);
            this.students = response.data;
        })
        axios.get("https://api.privatbank.ua/p24api/pubinfo?exchange&json&coursid=4").then((response)=>{
            console.log(response.data);
            this.currency = response.data;
        })
        
    
    },
    methods:{
        updateStudent:function(){
            Vue.axios.put("http://46.101.212.195:3000/students/"+this.studId, {
               mark: this.mark1,
                isDonePr: this.isDonePr1,
                  name: this.name1,
                group: this.group1,
            })
            axios.get("http://46.101.212.195:3000/students").then((response)=>{
                this.students = response.data;
            })
        },
        getData: function(id,mark, isDone,name,group){
            this.studId = id;
            this.mark1 = mark;
           this.isDonePr1 = isDone;
            this.name1 = name;
            this.group1 = group;
            this.showInput = true;
        },
        addStudent:function(){
            Vue.axios.post("http://46.101.212.195:3000/students", {
               
            isDonePr: this.isDonePr,
            name: this.name,
            group: this.group,
            mark:this.mark

            })
            axios.get("http://46.101.212.195:3000/students").then((response) => {
                this.students = response.data;
            }) },

            deleteStudent:function(id){
            Vue.axios.delete("http://46.101.212.195:3000/students/"+id, {
            })
            axios.get("http://46.101.212.195:3000/students").then((response)=>{
                this.students = response.data;
            })
            },

        

       deleteRow:function(id){
            this.students =  this.students.filter(stud => stud.id!=id)
       },
       convert:function(){
           for(let i=0; i<this.currency.length; i++){
               if (this.currency[i].ccy==this.start_ccy)
                     this.sell=this.currency[i].sale;
               if (this.currency[i].ccy==this.end_ccy)
                     this.buy=this.currency[i].buy;
           }
           this.end_value=(this.start_value*this.sell)/this.buy;
           this.result = this.start_value + " " + this.start_ccy + "  ===  " + this.end_value + " " + this.end_ccy;
           
       }
    }
}


</script>
