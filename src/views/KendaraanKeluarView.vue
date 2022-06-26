<script>

export default {

  name: "KendaraanKeluarView",
  data: function () {
    return {
      data: [],
      selectedIndex: -1,
      isLoading: false,
      formEdit: {
        jenis: "",
        no_polisi: "",
        blok:"",
        waktu_masuk: new Date().toISOString().slice(0, 10),
        status:"",
      },
      formAdd: {
        jenis: 0,
        no_polisi: "",
        blok:"",
        waktu_masuk: new Date().toISOString().slice(0, 10),
        status: "",
      },
    };
  },
  mounted() {
    this.load();
  },
  methods: {
    load() {
      // this.isLoading = true;
      fetch(`http://localhost/parkir_php/pages/jeniskendaraan/index.php`, {
        method: "GET",
      })
          .then((response) => {
            if (response.ok) {
              return response.json();
            }
          })
          .then((json) => {
            console.log(json.data);
            this.data = json.data;
            console.log(this.data);
          })
          .catch(() => {
            alert("terjadi error");
          })
          .finally(() => {
            this.isLoading = false;
          });
    },
    remove() {
      this.closeModal("modal-delete");
      if (this.selectedIndex >= 0) {
        const selectedData = this.data[this.selectedIndex];
        const payload = new URLSearchParams({
          jenis: selectedData.jenis,
        });
        fetch("http://localhost/parkir_php/pages/jeniskendaraan/delete.php", {
          method: "DELETE",
          headers: {
            "Content-Type": "application/x-www-form-urlencoded",
            "API-Key": "secret",
          },
          body: payload.toString(),
        })
            .then((response) => {
              if (response.ok) {
                this.data.splice(this.selectedIndex, 1);
              }
              return response.json();
            })
            .then((json) => {
              console.log(json);
              if (!json.status) {
                alert("Error");
              } else {
                alert("Sukses!");
              }
            })
            .catch(() => {
              alert("Terjadi error");
            });
      }
      this.selectedIndex = -1;
    },
    update() {
      this.closeModal("modal-update");
      if (this.selectedIndex >= 0) {
        const payload = new URLSearchParams({
          jenis: this.formEdit.jenis,
          no_polisi: this.formEdit.no_polisi,
          blok: this.formEdit.blok,
          waktu_masuk: this.formEdit.waktu_masuk,
          status: this.formEdit.status,
        });

        fetch(`http://localhost/parkir_php/pages/jeniskendaraan/update.php`, {
          method: "PATCH",
          headers: {
            "Content-Type": "application/x-www-form-urlencoded",
            "API-Key": "secret",
          },
          body: payload.toString(),
        })
            .then((response) => {
              return response.json();
            })
            .then((json) => {
              if (!json.status) {
                alert(json.error);
              } else {
                this.data[this.selectedIndex] = json.data;
              }
            })
            .catch((e) => {
              alert("Terjadi error" + e.toString());
            })
            .finally(() => {
              this.selectedIndex = -1;
            });
      }
    },

    showDelete(index) {
      this.showModal("modal-delete");
      this.selectedIndex = index;
    },
    showKonfirmasi(index) {
      this.showModal("modal-konfirmasi");
      this.selectedIndex = index;
      const selectedData = this.data[index];
      this.formEdit.jenis = selectedData.jenis;
      this.formEdit.no_polisi = selectedData.no_polisi;
      this.formEdit.blok = selectedData.blok;
      this.formEdit.waktu_masuk = selectedData.waktu_masuk;
      this.formEdit.status = selectedData.status;
      nextTick(() => {
        document.getElementById("nama_update").focus();
      });
    },
    showAdd() {
      this.showModal("modal-add");
      nextTick(() => {
        document.getElementById("nama_add").focus();
      });
    },
    addNew() {
      this.closeModal("modal-add");
      const payload = new URLSearchParams({
        jenis: this.formAdd.jenis,
        no_polisi: this.formAdd.no_polisi,
        blok: this.formAdd.blok,
        waktu_masuk: this.formAdd.waktu_masuk,
        status: this.formAdd.status,
      });

      fetch(`http://localhost/parkir_php/pages/jeniskendaraan/insert.php`, {
        method: "POST",
        headers: {
          "Content-Type": "application/x-www-form-urlencoded",
          "API-Key": "secret",
        },
        body: payload.toString(),
      })
          .then((response) => {
            return response.json();
          })
          .then((json) => {
            if (!json.status) {
              alert(json.error);
            } else {
              this.data.push(json.data);
            }
          })
          .catch((e) => {
            alert("Terjadi error" + e.toString());
          })
          .finally(() => {
            this.formAdd.nama = "";
          });
    },
    showModal(idbarang) {
      document.getElementById(idbarang).classList.add("is-active");
    },
    closeModal(idbarang) {
      document.getElementById(idbarang).classList.remove("is-active");
    },
  },

  setup() {
    return { collapsed, toggleSidebar };
  },
};
</script>

<template>
  <SideBar />
  <section class="content">
    <div class="container">
      <table class="table">
        <thead>
        <tr>
          <th>#</th>
          <th>Jenis Kendaraan</th>
          <th>Nomor Polisi</th>
          <th>Blok</th>
          <th>Waktu Masuk</th>
          <th>Status</th>
        </tr>
        </thead>
        <tbody v-if="data.length">
        <tr v-for="(value, index) in data" v-bind:key="index">
          <td>{{ index + 1 }}</td>
          <td>{{ value.jenis}}</td>
          <td>{{ value.no_polisi }}</td>
          <td>{{ value.blok }}</td>
          <td>{{ value.waktu_masuk }}</td>
          <td>{{ value.status}}</td>
          <td>
            <div class="field has-addons">
              <p class="control">
                <router-link
                    class="button is-success"
                    :to="{
                      name: 'detail',
                      params: { jenis: value.jenis},
                    }"
                >
                    <span class="icon is-small">
                      <i class="fas fa-arrow-right"></i>
                    </span>
                  <span>Konfirmasi</span>
                </router-link>
              </p>
              <p class="control">
                <button class="button is-warning" @click="showKonfirmasi(index)">
                    <span class="icon is-small">
                      <i class="fas fa-pencil"></i>
                    </span>
                  <span>Konfirmasi</span>
                </button>
              </p>
              <p class="control">
                <button class="button is-danger" @click="showDelete(index)">
                    <span class="icon is-small">
                      <i class="fas fa-trash"></i>
                    </span>
                  <span>Delete</span>
                </button>
              </p>
            </div>
          </td>
        </tr>
        </tbody>
        <div class="notif" v-else>
          <div class="notification is-warning">Tidak ada data</div>
        </div>
      </table>
    </div>
    <div class="has-text-centered" v-if="isLoading">
      <i class="fa-solid fa-spinner fa-pulse"></i>
    </div>
  </section>
  <div class="modal" id="modal-delete">
    <div class="modal-background"></div>
    <div class="modal-card">
      <header class="modal-card-head">
        <p class="modal-card-title">Delete</p>
      </header>
      <section class="modal-card-body">
        <div v-if="selectedIndex > -1">
          <ul>
            <li>
              ID <strong>{{ data[selectedIndex].jenis}}</strong>
            </li>
            <li>
              NAMA <strong>{{ data[selectedIndex].no_polisi }}</strong>
            </li>
            <li>
              MERK<strong>{{ data[selectedIndex].blok }}</strong>
            </li>
            <li>
              JUMLAH<strong>{{ data[selectedIndex].waktu_masuk }}</strong>
            </li>
            <li>
              HARGA<strong>{{ data[selectedIndex].status}}</strong>
            </li>
          </ul>
        </div>
      </section>
      <footer class="modal-card-foot">
        <button class="button is-danger" @click="remove">Delete</button>
        <button class="button" @click="closeModal('modal-delete')">
          Close
        </button>
      </footer>
    </div>
  </div>
  <div class="modal" id="modal-update">
    <div class="modal-background"></div>
    <div class="modal-card">
      <header class="modal-card-head">
        <p class="modal-card-title">Update</p>
      </header>
      <section class="modal-card-body">
        <div v-if="selectedIndex > -1">
          <form @submit.prevent="update">
            <div class="field">
              <label class="label" for="nama_update">Nama kategori</label>
              <div class="control">
                <input
                    id="nama_update"
                    class="input"
                    type="text"
                    placeholder="Nama kategori"
                    required
                    v-model="formEdit.namabarang"
                />
              </div>
            </div>
          </form>
        </div>
      </section>
      <footer class="modal-card-foot">
        <button class="button is-success" @click="update">Update</button>
        <button class="button" @click="closeModal('modal-update')">
          Close
        </button>
      </footer>
    </div>
  </div>
</template>
<style scoped>
.notif {
  min-width: 100%;
}
</style>
