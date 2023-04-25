<template>
    <div class="container mt-5">
  <div class="row justify-content-center">
    <div class="col-md-8">
      <div class="card shadow-lg rounded-3">
        <div class="card-header bg-primary text-white">
          <h3 class="card-title mb-0 text-center">User List</h3>
        </div>
        <div class="card-body">
          <div class="row g-4">
            <div class="col-lg-4">
              <div class="card bg-success text-white h-100">
                <div class="card-body d-flex align-items-center justify-content-center">
                  <i class="fas fa-users fa-4x"></i>
                  <div class="ms-3">
                    <h4 class="card-title">{{ midterms.length }}</h4>
                    <p class="card-text mb-0">All Users</p>
                  </div>
                </div>
              </div>
            </div>
            <div class="col-lg-4">
              <div class="card text-white h-100" style="background-color: pink;">
                <div class="card-body d-flex align-items-center justify-content-center">
                  <i class="fas fa-female fa-4x"></i>
                  <div class="ms-3">
                    <h4 class="card-title">{{ femaleUsers }}</h4>
                    <p class="card-text mb-0">Female Users</p>
                  </div>
                </div>
              </div>
            </div>
            <div class="col-lg-4">
              <div class="card text-white h-100" style="background-color: blue;">
                <div class="card-body d-flex align-items-center justify-content-center">
                  <i class="fas fa-male fa-4x"></i>
                  <div class="ms-3">
                    <h4 class="card-title">{{ maleUsers }}</h4>
                    <p class="card-text mb-0">Male Users</p>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="table-responsive mt-5">
            <table class="table table-striped table-hover table-bordered">
              <thead class="thead-dark">
                <tr>
                  <th class="text-center align-middle">Full Name</th>
                  <th class="text-center align-middle">Address</th>
                  <th class="text-center align-middle">Contact Number</th>
                  <th class="text-center align-middle">Gender</th>
                  <th class="text-center align-middle">Email</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(midterm, index) in midterms" :key="index" :class="{ 'table-danger': midterm.gender === 'Female', 'table-primary': midterm.gender === 'Male' }">
                  <td class="align-middle font-weight-bold">{{ midterm.fullname }}</td>
                  <td class="align-middle">{{ midterm.address }}</td>
                  <td class="align-middle">{{ midterm.contactnum }}</td>
                  <td class="align-middle">{{ midterm.gender }}</td>
                  <td class="align-middle font-italic">{{ midterm.email }}</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

  </template>
  
    
    <script setup>
    import { ref, onMounted } from 'vue'
    import {db} from '../Firebase/index.js'
    import {addDoc, collection, onSnapshot,deleteDoc, doc, updateDoc, getDoc, query, orderBy} from "firebase/firestore";
    import { auth } from '../Firebase/index.js'
    import { createUserWithEmailAndPassword } from 'firebase/auth'
    import { RouterLink } from 'vue-router'
    import router from '../router'
    
    
    
    const midterms = ref([])
    const id = ref(1)
    const q = (collection(db, "midterm"));
    
    const femaleUsers = ref(0)
    const maleUsers = ref(0)
    onMounted(async()=>{
      onSnapshot(q,(snapshot)=> {
        const midtermTmp = []
        let femaleCount = 0
        let maleCount = 0
        snapshot.forEach((doc) => {
    
        const midterm = {
          id: doc.id,
          fullname: doc.data().fullname,
          address: doc.data().address,
          gender: doc.data().gender,
          email: doc.data().email,
          contactnum: doc.data().contactnum,
          completed: doc.data().completed
        }
    
        midtermTmp.push(midterm)
        if(midterm.gender == 'Female'){
            femaleCount++
        }
        if(midterm.gender == 'Male'){
            maleCount++
        }
      });
      midterms.value = midtermTmp
      femaleUsers.value = femaleCount
      maleUsers.value = maleCount
      })
    })
    </script>