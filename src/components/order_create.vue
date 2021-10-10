<template>
  <v-container class="main">
    <!-- <p>{{ form.customers }}</p> -->
    <!-- <v-btn @click="searching">Fetch</v-btn> -->

    <v-card style="padding: 30px">
      <v-card-title>Customer Service</v-card-title>
      <v-autocomplete
        v-model="form.customers"
        :items="items"
        :loading="isLoading"
        :search-input.sync="search"
        color="white"
        item-text="Description"
        item-value="API"
        placeholder="Customer Name"
        return-object
      ></v-autocomplete>
      <v-text-field
        label="Product Weight"
        placeholder="Product Weight"
        v-model="form.product"
      ></v-text-field>
      <v-text-field
        label="Box Weight"
        placeholder="Box Weight"
        v-model="form.box"
      ></v-text-field>
      <v-text-field
        label="Total Weight"
        placeholder="Total Weight"
        v-model="form.total"
      ></v-text-field>
      <v-row style="margin: 10px" justify="end">
        <v-dialog
          width="350"
          v-model="submitDialog"
          persistent
          transition="dialog-bottom-transition"
        >
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              v-bind="attrs"
              v-on="on"
              style="margin-right: 5px; color: white"
              color="green"
              width="100"
              >Submit</v-btn
            >
          </template>
          <v-card>
            <v-card-title>Submission Customer</v-card-title>
            <div style="text-align: center">
              <p>Do you want to submit order?</p>
            </div>
            <v-divider></v-divider>
            <v-card-actions style="display: flex; justify-content: flex-end">
              <v-btn
                width="100"
                color="green"
                style="color: white"
                @click="generateReport()"
                >Confirm</v-btn
              >
              <v-btn
                width="100"
                color="grey"
                style="color: white"
                @click="submitDialog = false"
                >Cancel</v-btn
              >
            </v-card-actions>
          </v-card>
        </v-dialog>

        <v-btn
          style="margin-left: 5px; color: white"
          color="red"
          width="100"
          @click="onReset"
          >Reset</v-btn
        >
      </v-row>
    </v-card>
    <v-snackbar v-model="snackbar" timeout="500">
      Create Order Complete

      <template v-slot:action="{ attrs }">
        <v-btn color="pink" text v-bind="attrs" @click="snackbar = false">
          Close
        </v-btn>
      </template>
    </v-snackbar>

    <vue-html2pdf
      :show-layout="false"
      :float-layout="true"
      :enable-download="true"
      :preview-modal="true"
      :paginate-elements-by-height="1400"
      :pdf-quality="2"
      :manual-pagination="false"
      pdf-format="a8"
      pdf-orientation="landscape"
      pdf-content-width="500px"
      ref="html2Pdf"
    >
      <section slot="pdf-content">
        <v-container>
          <br />
          <h5>ชื่อลูกค้า : {{ form.customers }}</h5>
          <h5>น้ำหนักทอง :{{ form.product }} กรัม</h5>
          <h5>น้ำหนักกล่อง : {{ form.box }} กรัม</h5>
          <h5>น้ำหนักรวม : {{ form.total }} กรัม</h5>
        </v-container>
      </section>
    </vue-html2pdf>
  </v-container>
</template>

<script>
import VueHtml2pdf from "vue-html2pdf";
import axios from "axios";

export default {
  name: "CreateOrder",
  components: { VueHtml2pdf },
  data: () => ({
    snackbar: false,
    submitDialog: false,
    descriptionLimit: 60,
    entries: [],
    isLoading: false,
    model: null,
    search: null,
    form: {
      product: "",
      box: "",
      total: "",
      customers: null,
    },
  }),
  methods: {
    closeDialog() {
      this.submitDialog = false;
    },
    onSubmit(event) {
      event.preventDefault();
      alert(JSON.stringify(this.form));
    },
    onReset(event) {
      event.preventDefault();
      // Reset our form values
      this.form.customer = [];
      this.form.product = "";
      this.form.box = "";
      this.form.total = "";
      // Trick to reset/clear native browser form validation state
      this.show = false;
      this.$nextTick(() => {
        this.show = true;
      });
    },
    async generateReport() {
      await this.$refs.html2Pdf.generatePdf();
      this.closeDialog();
      this.snackbar = true;
    },
  },

  computed: {
    items() {
      console.log(this.entries);
      return this.entries.map((element) => element.name);
    },
  },

  watch: {
    async search(val) {
      this.isLoading = true;
      console.log("Seatch Key ");
      let url = `https://delivery-api.shininggold.co.th/search_partner/${val}`;
      await axios
        .get(url)
        .then((res) => (this.entries = res.data.data))
        .catch((err) => console.log(err));
      this.isLoading = false;
    },
  },
};
</script>

<style scoped>
.main {
  padding: 5%;
}
</style>
