<template>
  <div class="container">
        <div class="row">
            <div class="col-md-6 col-md-offset-3 jumbotron">

                <div class="form-group">
                    <label for="name" class="name">Name:</label>
                    <input type="text" class="form-control" v-validate="'required'" v-model="signup.name" id="name" placeholder="Enter name" name="name">
                    <em class="text-danger">{{errors.first('name')}}</em>
                </div>

                <div class="form-group">
                    <label for="email" class="email">Email:</label>
                    <input type="text" v-validate="'required|email'" class="form-control" id="email" v-model="signup.email" placeholder="Enter email" name="email">
                   <em class="text-danger" >{{errors.first('email')}}</em>
                </div>
                
                <div class="form-group">
                    <label for="phone" class="phone">Phone number:</label>
                    <input type="number" v-validate="'required|numeric'" class="form-control" id="email" v-model="signup.phone" placeholder="Enter phone" name="phone">
                   <em class="text-danger" >{{errors.first('phone')}}</em>
                </div>

                 <div class="form-group">
                    <label for="address" class="address">Address:</label>
                    <input type="text" v-validate="'required'" class="form-control" id="address" v-model="signup.address" placeholder="Enter address" name="address">
                   <em class="text-danger" >{{errors.first('address')}}</em>
                </div>
                
                <div class="form-group">
                    <label for="zipcode" class="zipcode">Zip Code:</label>
                    <input type="number" v-validate="'required|numeric|max:5'" maxlength="5" class="form-control" id="zipcode" v-model="signup.zipcode" placeholder="Enter zip code" name="zipcode">
                   <em class="text-danger" >{{errors.first('zipcode')}}</em>
                </div>
                <div class="form-group">
                    <label for="file" class="file">Upload Image or document:</label>
                    <input type="file" v-validate="'required'"  class="form-control" id="file" name="file" ref="file" @change="handleFileUpload()" >
                   <em class="text-danger" >{{errors.first('file')}}</em>
                </div>
                <div class="login_button">
                    <button type="submit" class="form-control btn btn-primary" @click="signUp()">Sign Up</button>
                </div>

            </div>
        </div>

    </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'Signup',
  data(){
      return{
          APP_BASE_URL:'http://localhost:3000',
          signup:{
              name:'',
              email:'',
              zipcode:'',
              phone:'',
              address:'',
              file:''
          },
      }
  },
  methods:{

        handleFileUpload(){
            this.signup.file = this.$refs.file.files[0];
            
        },
      signUp(){
                this.$validator.validateAll().then(result =>{

                    let formData = new FormData();
                    appendFormdata(formData,this.signup);
                   if(result){
                      axios({
                        method: 'post',
                        url: 'http://localhost/assignment/public/api/signup',
                        data:formData,
                        headers: { 
                            'Accept': 'application/json',
                             'Content-Type': 'application/x-www-form-urlencoded; charset=UTF-8',
                        },
                        })
                        .then(response => {
                            if(response.status == 200){
                                alert('created successfully');
                            }
                            else{
                            console.log(response);
                            }
                        });

                    }

                }).catch(error=>{
                    console.log(error);
                })
      }
  }
}
</script>

<style scoped>
        .form-control {
            height: 42px;
        }

        .form-group {
            padding: 5px;
        }

        .login_button {
            display: flex;
            justify-content: center;
        }

        .btn-primary {
            width: 25%;
        }

        .navbar-default .navbar-nav>li>a {
            font-size: 16px;
        }
</style>