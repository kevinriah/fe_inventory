<template>
  <CModal
    :show.sync="modal"
    @update:show="onClose"
    color="primary"
    :closeOnBackdrop="false"
    title="Daftar Barang"
  >
    <CRow>
      <CCol>
        <data-table
          :buttonadd="false"
          :headers="header"
          :items="temp"
          @selected="onBarangSelected"
        >
          <template #search>  
            <CForm inline>
              <CInput
                v-model="input"
                placeholder="Search"
                @keypress="search"
                size="sm"
              />
            </CForm>
          </template>
        </data-table>
      </CCol>
    </CRow>
  </CModal>
</template>

<script>
import API from "../services/api.service";
export default {
  name: "dialog_pick_barang",
  props: ["show"],
  data: () => {
    return {
      modal: false,
      header: [
        { key: "nama_barang", label: "Nama Barang" },
        { key: "nama_satuan", label: "Satuan" },
        { key: "harga_modal", label: "Harga Beli" },
        { key: "harga_jual", label: "Harga Jual" },
      ],

      listBarang: [],
      temp: [],
      input: "",
    };
  },
  watch: {
    show: function (newval, old) {
      console.log(newval);
      this.modal = newval;
    },
  },
  created() {
    this.getBarang();
  },
  methods: {
    getBarang() {
      API.get("masterbarangcontroller").then(({ status, data }) => {
        if (status == 200 || status == 201) {
          if (data.status) {
            this.listBarang = data.data;
            this.temp = data.data;
          }
        }
      });
    },
    search() {
      this.temp = [];
      if (this.input.length < 1) {
        this.temp = this.listBarang;
      }
      let data = this.listBarang.filter((barang) => {
        return barang.nama_barang.toLowerCase().includes(this.input.toLowerCase());
      });
      this.temp = data;
    },
    onBarangSelected(data) {
      this.$emit("onselected", data);
    },
    onClose(event) {
      console.log(event);
      this.$emit("action", event);
    },
  },
};
</script>

<style scoped></style>
