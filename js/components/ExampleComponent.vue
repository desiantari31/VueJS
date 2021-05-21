<template>
    <div class="container mt-4">
       <form @submit.prevent="submit">
           <div>
               <label for="hari" class="mr-4">Hari</label>
               <input type="text" v-model="form.hari">
           </div>
           <div>
               <label for="mata_pelajaran" class="mr-2">Mata Pelajaran</label>
               <input type="text" v-model="form.mata_pelajaran">
           </div>
           <div>
               <label for="kelas" class="mr-2">Kelas</label>
               <input type="text" v-model="form.kelas">
           </div>
           <input type="submit" value="submit">
       </form>
       <div><br>
           <table>
               <tr>
                   <th>Hari</th>
                   <th>Mata Pelajaran</th>
                   <th>Kelas</th>
                   <th>Action</th>
               </tr>
               <tr v-for="mapel in mapels" :key="mapel.id">
                   <td>{{mapel.hari}}</td>
                   <td>{{mapel.mata_pelajaran}}</td>
                   <td>{{mapel.kelas}}</td>
                   <td>
                        <button @click="edit(mapel.id)">Edit</button>
                        <button @click="remove(mapel.id)">Delete</button>
                    </td>
               </tr>
           </table>
       </div>
    </div>
</template>

<script>
import axios from 'axios'
    export default {
        name: 'ExampleComponent',
        data:()=>({
            form:{
                hari:null,
                mata_pelajaran:null,
                kelas:null,
            },
            mapels:null,
        }),
        methods:{
            submit(){
                axios.post('http://127.0.0.1:8000/api/mapels/', this.form).then(res=>{
                    console.log("res from api", res.data)
                }).catch(err=>{
                    console.log("error ", err.response)
                })
            },
            get(){
                axios.get('http://127.0.0.1:8000/api/mapels').then(res=>{
                    console.log(">> ", res.data)
                    this.posts = res.data
                }).catch(err=>{
                    console.log("error ", err.response)
                })
            },
            edit(id){
                axios.get('http://127.0.0.1:8000/api/mapels/'+id).then(res=>{
                    this.form.hari = res.data.nis
                    this.form.mata_pelajaran = res.data.nama
                    this.form.kelas = res.data.email
                })
            },
            update(id){
                axios.put('http://127.0.0.1:8000/api/mapels/'+id, this.form).then(res=>{
                    console.log("res ", res.data)
                    this.get();
                }).catch(err=>{
                    console.log("error ", err.response)
                })
            },
            remove(id){
                axios.delete('http://127.0.0.1:8000/api/mapels/'+id).then(res=>{
                    console.log("deleted", res.data)
                    this.get();
                }).catch(err=>{
                    console.log("error ", err.response)
                })
            }
        },
        created(){
            this.get();
        },
        mounted() {
            console.log('Component mounted.')
        }
    }
</script>
<style lang="scss" scoped>
table,tr,th,td{
    border: 1px solid #ddd;
}
</style>