<template>
  <div>
    <CRow>
      <CCol lg="12">
        <data-table
          :items="items"
          :headers="header"
          title="Barang Keluar"
          @edit="editPenjualan"
          @delete="deletePenjualan"
        >
          <template #tambah>
            <CButton
              @click="$router.push({ path: '/master/addpenjualan' })"
              color="primary"
            >
              Tambah Data
            </CButton> </template
          ><template #search
            ><CForm inline>
              <CInput class="ml-12 sm-6" placeholder="Search" size="sm" v-model="katakunci" />

              <CButton color="outline-success" class="" type="button" @click="cariDataPenjualan"
                >Search</CButton
              >
            </CForm></template
          >
        </data-table>
      </CCol>
    </CRow>
  </div>
</template>

<script>
import API from "../../services/api.service";

export default {
  name: "Penjualan",
  data: () => {
    return {
      header: [
        { key: "nomor_nota", label: "Nomor Nota" },
        { key: "tanggal", label: "Tanggal" },
        { key: "subtotal", label: "Sub Total" },
        { key: "actions", label: "Aksi" },
      ],
      hidden: false,
      katakunci: '',
      items: [],
    };
  },
  created() {
    this.getDataPenjualan();
  },
  methods: {
    getDataPenjualan() {
      API.get("penjualancontroller")
        .then(({ status, data }) => {
          if (status === 200 || status === 201) {
            if (data.status) {
              this.items = data.data;
            }
          }
        })
        .catch(() => {});
    },
    cariDataPenjualan() {
      API.get("penjualanancontroller/search?nomor_nota="+this.katakunci).then(({status,data}) => {
        if(status == 200 || status == 201){
          if(data.status){
            this.items = data.data
          }
        }
      })
      .catch(() => {});
    },
    editPenjualan(data) {
      this.$router.push({ path: "/master/editpenjualan/" + data.id_penjualan });
    },
    deletePenjualan(dataPenjualan) {
      console.log(dataPenjualan);
      API.delete(`penjualancontroller/${dataPenjualan.id_penjualan}`)
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
              this.getDataPenjualan(); // untuk me reload data ke back end
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
