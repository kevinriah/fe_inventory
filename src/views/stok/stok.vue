<template>
  <div>
    <CRow>
      <CCol lg="12">
        <data-table
          :items="items"
          :headers="header"
          title="Stok Barang"
          @edit="editStok"
          @delete="deleteStok"
        >
        <template #search
            ><CForm inline>
              <CInput class="ml-12 sm-6" placeholder="Search" size="sm" v-model="katakunci"/>

              <CButton color="outline-success" class="" type="button" @click="cariDataStok"
                >Search</CButton
              >
            </CForm></template
          >
        </data-table>
        <CPagination align="center" :pages="10" />
      </CCol>
    </CRow>
  </div>
</template>

<script>
import API from "../../services/api.service";

export default {
  name: "StokBarang",
  data: () => {
    return {
      header: [
        { key: "kode_barang", label: "Kode Barang" },
        { key: "nama_barang", label: "Nama Barang" },
        { key: "nama_satuan", label: "Satuan" },
        { key: "stok", label: "Kuantitas" },
        //{ key: "actions", label: "Aksi" },
      ],
      hidden: false,
      katakunci: '',
      items: [],
    };
  },
  created() {
    this.getDataStok();
  },
  methods: {
    getDataStok() {
      API.get("stokbarangcontroller").then(({ status, data }) => {
        if (status === 200 || status === 201) {
          if (data.status) {
            this.items = data.data;
          }
        }
      });
    },
    cariDataStok(){
      API.get("stokbarangcontroller/search?nama_barang="+this.katakunci).then(({status,data}) => {
        if(status == 200 || status == 201){
          if(data.status){
            this.items = data.data
          }
        }
      })
      .catch(() => {});
    },
    editStok(data) {
      this.$router.push({ path: "/master/editstok/" + data.id_stok_barang });
    },
    deleteStok(dataStok) {
    console.log(dataStok);
      API.delete(`stokbarangcontroller/${dataStok.id_stok_barang}`)
        .then(({ status, data }) => {
          if (status === 200 || status === 201) {
            if (data.status) {
              this.$notify({
                // ketika data berhasil dihapus maka muncul notif
                group: "notif",
                type: "success",
                title: "Informasi",
                text: "Data telah berhasil dihapus",
              });
              this.getDataStok(); 
            } else {
              // ketika data gagal dihapus maka muncul notif
              this.$notify({
                group: "notif",
                type: "error",
                title: "Perhatian",
                text: "Data gagal untuk dihapus",
              });
            }
          } else {
            // ketika data gagal dihapus maka muncul notif
            this.$notify({
              group: "notif",
              type: "error",
              title: "Perhatian",
              text: "Data gagal untuk dihapus",
            });
          }

          //tambahin ini buat notif ketika error 500 dll dari back end
        })
        .catch(() => {
          this.$notify({
            group: "notif",
            type: "error",
            title: "Perhatian",
            text: "Data gagal untuk dihapus",
          });
        });
    },
  },
};
</script>
