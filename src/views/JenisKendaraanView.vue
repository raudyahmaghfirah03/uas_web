<template>
  <form>
    <div class="mb-3">
      <label for="inputJeniskendaraan" class="form-label">Jenis Kendaraan</label>
      <input type="text" class="form-control" id="inputJeniskendaraan" v-model="addJenisKendaraan.jenis_kendaraan"/>
    </div>
    <div class="mb-3">
      <label for="inputBayar" class="form-label">Bayar</label>
      <input type="number" class="form-control" id="inputBayar" v-model="addJenisKendaraan.bayar "/>
    </div>
    <div class="mb-3">
      <label for="inputBlok" class="form-label">Blok</label>
      <input type="text" class="form-control" id="inputBlok" v-model="addJenisKendaraan.blok"/>
    </div>

    <button type="jeniskendaraan" class="btn btn-primary" @click="jeniskendaraanAdd">ADD</button>
  </form>

</template>

<script>
export default {
  name: "JenisKendaraanView",
  data() {
    return {
      data: [],
      addJenisKendaraan: {
        jenis_kendaraan: '',
        bayar: 0,
        blok : '',
      }
    }
  },
  methods: {
    jeniskendaraanAdd() {
      const formJeniskendaraan = new URLSearchParams({
        jenis_kendaraan: this.addJenisKendaraan.jenis_kendaraan,
        bayar: this.addJenisKendaraan.bayar,
        blok: this.addJenisKendaraan.blok,


      });
      fetch(`http://localhost/parkir_php/pages/jeniskendaraan/create.php`,{
        method:'POST',
        headers:{
          'Content-Type': 'application/x-www-form-urlencoded',
          'API-Key': 'secret'
        },
        body: formJeniskendaraan.toString(),
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
            this.addJenisKendaraan.jenis_kendaraan='';
            this.addJenisKendaraan.bayar='';
            this.addJenisKendaraan.blok='';
          })

    }
  }
};
</script>

<style scoped></style>
