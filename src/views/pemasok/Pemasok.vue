<template>
  <div>
    <CRow >
      <CCol lg="12">
        <data-table 
        :items="items" 
        :headers="header" 
        title="Pemasok" 
        @edit="editPemasok" 
        @delete="deletePemasok"
        >
          <template #tambah>
            <CButton
              @click="$router.push({ path: '/master/addpemasok' })"
              color="primary"
            >
              Tambah Data
            </CButton> </template
          ><template #search
            ><CForm inline>
              <CInput class="ml-12 sm-6" placeholder="Search" size="sm" v-model="katakunci" />

              <CButton color="outline-success" class="" type="button" @click="cariDataPemasok"
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
  name: "Pemasok",
  data: () => {
    return {
      header: [
        { key: "nama_pemasok", label: "Nama Pemasok" },
        { key: "alamat", label: "Alamat" },
        { key: "kota", label: "Kota" },
        { key: "telepon", label: "Nomor Telepon" },
        { key: "actions", label: "Aksi" },
      ],
      hidden:false,
      katakunci:'',
      items: [],
    };
  },
  created() {
    this.getDataPemasok();
  },
  methods: {
    getDataPemasok() {
      API.get("pemasokcontroller").then(({status,data}) => {
        if(status == 200 || status == 201){
          if(data.status){
            this.items = data.data
          }
        }
      })
      .catch(() => {});
    },
    cariDataPemasok() {
      API.get("pemasokcontroller/search?nama_pemasok="+this.katakunci).then(({status,data}) => {
        if(status == 200 || status ==201){
          if(data.status){
            //console.log(data);
            this.items = data.data
          }
        }
      })
      .catch(() => {});
    },
    editPemasok(data){
      this.$router.push({path: "/master/editpemasok/"+data.id_pemasok }) 
    },
    deletePemasok(dataPemasok) {
    console.log(dataPemasok);
      API.delete(`pemasokcontroller/${dataPemasok.id_pemasok}`).then(({status,data}) => {
        if(status == 200 || status == 201){
          if(data.status){
            this.$notify({ 
            // ketika data berhasil dihapus maka muncul notif
              group: "notif",
              type: "success",
              title: "Informasi",
              text: "Data telah berhasil dihapus",
            });
            this.getDataPemasok();
          }
          else 
          { 
          // ketika data gagal dihapus maka muncul notif
          this.$notify({
              group: "notif",
              type: "error",
              title: "Perhatian",
              text: "Data gagal untuk dihapus",
            });
          }

        }
        else
        { 
        // ketika data gagal dihapus maka muncul notif
          this.$notify({
              group: "notif",
              type: "error",
              title: "Perhatian",
              text: "Data gagal untuk dihapus",
            });
        }

        //tambahin ini buat notif ketika error 500 dll dari back end
      }).catch(()=>{
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
