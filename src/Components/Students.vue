<template>
    

   <div>

       <div class="theme-switches">
          <link rel="stylesheet" href="./style.css">
           <link rel="stylesheet" id="switcher-id" v-bind:href="chootheme == 'light' ? './light.css' : './dark.css'">
            <div data-theme="light" class="switch" id="switch-1" v-on:click="choseTheme('light')"></div>
            <div data-theme="dark" class="switch" id="switch-2" v-on:click="choseTheme('dark')"></div>
        </div>


      <table>
            {{studentsCount}}
                <tr>
                    <th>Photo</th>
                    <th>Name</th>
                    <th>Group</th>
                    <th>Mark</th>
                    <th>PR is Done</th>
                </tr>

                 <tr>
                <tr v-for="stud in students" v-bind:key="stud._id" >
                    <td><img v-bind:src="stud.photo"  width="50px"></td>
                    <td >
                        <div v-bind:style="idTest == stud._id ? 'display:none' : 'display:inline'"><router-link v-bind:to="'/student-info/'+stud._id">{{stud.name}}</router-link></div>
                        <input v-bind:style="idTest == stud._id ? 'display:inline' : 'display:none'"  v-model="newStudent.name">
                    </td>
                    <td>
                        <div v-bind:style="idTest == stud._id ? 'display:none' : 'display:inline'">{{stud.group}}</div>
                        <select input v-bind:style="idTest == stud._id ? 'display:inline' : 'display:none'" name="group" v-model="newStudent.group" >
                        <option value="RPZ 18 1/9">RPZ 18 1/9</option>
                        <option value="RPZ 18 2/9">RPZ 18 2/9</option>
                        </select>
                    </td>
                    <td>
                        <div v-bind:style="idTest == stud._id ? 'display:none' : 'display:inline'">{{stud.mark}}</div>
                        <input v-bind:style="idTest == stud._id ? 'display:inline' : 'display:none'" v-model="newStudent.mark">
                    </td>
                    <td>
                        
                        <div   type="checkbox" v-bind:style="idTest == stud._id ? 'display:none' : 'display:inline'" > <input type="checkbox" v-bind:checked="stud.isDonePr"></div>
                        <input type="checkbox" v-bind:style="idTest == stud._id ? 'display:inline' : 'display:none'" v-model="newStudent.isDonePr">
                    </td>
                    <td>
                        <td><a href="#" v-on:click.prevent="deleteStudent(stud._id)" v-show="stud.group==getCurrentUser.group">Видалити</a>
                    </td>
                    <td>
                        <button v-on:click="get(stud._id,stud.name,stud.group,stud.isDonePr,stud.mark)">
                            <img src="/1.png" width="25px">
                        </button>
                    </td>
                    <button v-on:click="update()" v-bind:style="showInput ? 'display:inline' : 'display:none'">edit</button>
                </tr>
            </table>



            <br> <h2>Add Student : </h2>
            <br> Name  <input v-model="name">
            <br> Group <input v-model="group">
            <br> Mark <input type="number"  v-model="mark">
            <br> PR <input type="checkbox" v-model="isDonePr">
            <br> <button v-on:click="add">Add</button>



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
            <br>{{result}}{{end_value | roundValue}}{{end_ccy}}
   </div>
</template>

<script>
    import VueRouter from 'vue-router'
import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'


    export default {
        props: {
           id: '',
       },

    data: function() {
        return {
        students: [],
        currency:[],
        newStudent: {},
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
        idTest:"",
        studId:"",
    }},
    mounted: function(){
        axios.get("http://46.101.212.195:3000/students").then((response)=>{
            console.log(response.data);
            this.students = response.data;
            this.$store.commit('setCount', this.students.length);
        })
        axios.get("https://api.privatbank.ua/p24api/pubinfo?exchange&json&coursid=4").then((response)=>{
            console.log(response.data);
            this.currency = response.data;
        })
        
    
    },

    computed:{
          studentsCount () {
    return this.$store.getters.getCount
        },
         chootheme(){
    return this.$store.getters.getTheme
         },
         getCurrentUser () {
                return this.$store.getters.getUser
         },

    },

    methods:{
       deletes:function(id){
         Vue.axios.delete("http://46.101.212.195:3000/students/"+id) 
         .then((response)=> {

             
            axios.get("http://46.101.212.195:3000/students").then((response)=>{
                this.students = response.data;
                this.$store.commit('setCount', this.students.length)
            })
            })
           
        },
         get: function(id,name,group,isDone,mark){
             this.newStudent.id = id;
            this.newStudent.name = name;
            this.newStudent.group = group;
            this.newStudent.isDonePr = isDone;
            this.newStudent.mark = mark;
            this.studId = id;
            this.idTest = id;
            this.showInput = true;
        },
        update:function(){
            Vue.axios.put("http://46.101.212.195:3000/students/"+this.studId, {
                name: this.newStudent.name,
                group: this.newStudent.group,
                isDonePr: this.newStudent.isDonePr,
                mark:this.newStudent.mark,
               
            })
            axios.get("http://46.101.212.195:3000/students").then((response)=>{
                this.students = response.data;
            })
            this.idTest = "";
        },
        
    add:function(){ 
            Vue.axios.post("http://46.101.212.195:3000/students", { 
                
                name: this.name, 
                group: this.group, 
                isDonePr: this.isDonePr, 
                mark:this.mark,
            }) 
            .then((response) => { 

                console.log(response.data)
                axios.get("http://46.101.212.195:3000/students").then((response)=>{ 
                this.students = response.data; 
                this.$store.commit('setCount', this.students.length)
            }) 
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
           this.result = this.start_value + " " + this.start_ccy + "  ===  " ;
           
       },
        choseTheme: function(theme){
                this.$store.commit('setTheme', theme);
            },
    },
    filters:{
            roundValue: function(value){
                return parseFloat(value.toFixed(2));
            },
        }

}


</script>
