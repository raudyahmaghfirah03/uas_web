<template>
  <form>
    <div class="mb-3">
      <label for="inputNamaUser" class="form-label">Nama Lengkap</label>
      <input type="email" class="form-control" id="inputNamaUser" v-model="addUser.nama_user"/>
    </div>
    <div class="mb-3">
      <label for="inputUsername" class="form-label">Username</label>
      <input type="email" class="form-control" id="inputUsername" v-model="addUser.username" />
    </div>
    <div class="mb-3">
      <label for="exampleInputPassword1" class="form-label">Password</label>
      <input type="password" class="form-control" id="exampleInputPassword1" v-model="addUser.password" />
    </div>
    <div class="mb-3">
      <label for="inputNoTelp" class="form-label">No. Telp</label>
      <input type="email" class="form-control" id="inputNoTelp" v-model="addUser.no_telp"/>
    </div>

    <div class="mb-3">
      <label for="exampleInputEmail1" class="form-label">Email address</label>
      <input
        type="email"
        class="form-control"
        id="exampleInputEmail1"
        aria-describedby="emailHelp"
      v-model="addUser.email"/>
      <div id="emailHelp" class="form-text">
        We'll never share your email with anyone else.
      </div>
    </div>
    <select class="form-select" aria-label="Default select example" v-model="addUser.jenis_kelamin">
      <option selected>Jenis Kelamin</option>
      <option value="laki-laki">Laki-Laki</option>
      <option value="perempuan">Perempuan</option>
    </select>

    <button type="register" class="btn btn-primary" @click="userAdd">Register</button>
  </form>
</template>

<script>
export default {
  name: "RegistereView",
  data() {
    return {
      data: [],
      addUser: {
        nama_user: '',
        username: '',
        password : '',
        no_telp : '',
        email: '',
        jenis_kelamin: '',
      }
    }
  },
  methods: {
    userAdd() {
      const formUser = new URLSearchParams({
        nama_user: this.addUser.nama_user,
        username: this.addUser.username,
        password: this.addUser.password,
        no_telp: this.addUser.no_telp,
        email: this.addUser.email,
        jenis_kelamin: this.addUser.jenis_kelamin
      });
      fetch(`http://localhost/parkir_php/pages/users/create.php`,{
        method:'POST',
        headers:{
          'Content-Type': 'application/x-www-form-urlencoded',
          'API-Key': 'secret'
        },
        body: formUser.toString(),
      })
          .then(response => {
            return response.json()
          })
          .then(json => {
            if(!json.status){
              alert(json.error);
            }else{
              this.data.push(json.data);
            }
          })
          .catch((e) =>{
            alert('Terjadi error' + e.toString())
          })
          .finally(()=>{
            this.addUser.nama_user='';
            this.addUser.no_telp='';
          })

    }
  }
};
</script>

<style scoped></style>
