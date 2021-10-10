<template>
  <div>
    <v-container>
      <b-form @submit="onSubmit" @reset="onReset" v-if="show">
        <b-form-group id="customer" label="Customer:" label-for="customer">
          <b-form-select
            id="customer"
            class="form-control"
            v-model="form.customer"
            :options="customers"
            required
          ></b-form-select>
        </b-form-group>
        <br />
        <b-form-group
          id="product_weight"
          label="ProductWeight:"
          label-for="product_weight"
        >
          <b-form-input
            id="product_weight"
            v-model="form.product"
            type="number"
            placeholder="ProductWeight"
            required
          ></b-form-input>
        </b-form-group>
        <br />

        <b-form-group id="box_weight" label="BoxWeight:" label-for="box_weight">
          <b-form-input
            id="box_weight"
            v-model="form.box"
            type="number"
            placeholder="BoxWeight"
            required
          ></b-form-input>
        </b-form-group>

        <br />
        <b-form-group
          id="total_weight"
          label="TotalWeight:"
          label-for="total_weight"
        >
          <b-form-input
            id="total_weight"
            v-model="form.total"
            type="number"
            placeholder="TotalWeight"
            required
          ></b-form-input>
        </b-form-group>

        <br />
        <b-button variant="success" @click="generateReport">Submit</b-button>
        <b-button type="reset" variant="danger">Reset</b-button>
      </b-form>
    </v-container>
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
          <h5>ชื่อลูกค้า : {{ form.customer }}</h5>
          <h5>น้ำหนักทอง :{{ form.product }} กรัม</h5>
          <h5>น้ำหนักกล่อง : {{ form.box }} กรัม</h5>
          <h5>น้ำหนักรวม : {{ form.total }} กรัม</h5>
        </v-container>
      </section>
    </vue-html2pdf>
  </div>
</template>

<script>
import VueHtml2pdf from "vue-html2pdf";
export default {
  data() {
    return {
      form: {
        product: "",
        box: "",
        total: "",
        customers: null,
      },
      customers: [
        { text: "กรุณาเลือก", value: null },
        { text: "ห้างทองเอกเซ่งเฮง", value: "ห้างทองเอกเซ่งเฮง" },
        { text: "ห้างทองทองดี", value: "ห้างทองทองดี" },
        { text: "ห้างทองจันทร์สม", value: "ห้างทองจันทร์สม" },
        { text: "ห้างทองโต๊ะกัง", value: "ห้างทองโต๊ะกัง" },
      ],
      show: true,
    };
  },
  methods: {
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
    generateReport() {
      this.$refs.html2Pdf.generatePdf();
    },
  },
  components: {
    VueHtml2pdf,
  },
};
</script>
