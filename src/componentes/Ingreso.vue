<template>
  <section class="src-componentes-ingreso">
    <div class="jumbotron">
      <h2>Ingreso de Gastos</h2>
      <hr />

      <vue-form :state="formstate" @submit.prevent="enviar()">
        <!-- Campo Nombre -->
        <validate tag="div">
          <label for="nombre">Nombre</label>
          <input
            type="text"
            id="nombre"
            v-model="formData.nombre"
            required
            name="nombre"
            autocomplete="off"
            class="form-control"
            minlength="3"
            maxlength="15"
            @keydown.space.prevent
          />

          <field-messages name="nombre" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">
              Este campo es obligatorio
            </div>
          </field-messages>
        </validate>

        <br />

        <!-- Campo Descripcion -->

        <validate tag="div">
          <label for="descripcion">Descripción</label>
          <input
            type="text"
            id="descripcion"
            v-model="formData.descripcion"
            required
            name="descripcion"
            autocomplete="off"
            class="form-control"
          />

          <field-messages name="descripcion" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">
              Este campo es obligatorio
            </div>
          </field-messages>
        </validate>

        <br />

        <!-- Campo Importe -->

        <validate tag="div">
          <label for="importe">Importe</label>
          <input
            type="number"
            id="importe"
            v-model.number="formData.importe"
            required
            name="importe"
            autocomplete="off"
            class="form-control"
          />

          <field-messages name="importe" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">
              Este campo es obligatorio
            </div>
          </field-messages>
        </validate>

        <br />

        <button
          class="btn btn-success mt-3"
          :disabled="formstate.$invalid"
          type="submit"
        >
          Enviar
        </button>
      </vue-form>

      <br />
      <hr />
      <!-- Tabla -->

      <h2>Detalle de Gastos</h2>
      <br />
      <div v-if="gastos.length" class="table-responsive">
        <h4>Presupuesto:</h4>
        <input
          :style="{ color: getColorPresupest }"
          type="number"
          name="presupuesto"
          v-model="presupuesto"
          placeholder="Ingrese el presupuesto"
        />
        <br />
        <table class="table">
          <tr>
            <th>Nombre</th>
            <th>Descripción</th>
            <th>Importe</th>
            <th>Fecha</th>
          </tr>
          <tr v-for="(gasto, index) in gastos" :key="index">
            <td>{{ gasto.nombre }}</td>
            <td>{{ gasto.descripcion }}</td>
            <td>$ {{ gasto.importe }}</td>
            <td>{{ gasto.fecha }}</td>
          </tr>
          <tr :style="{ color: calcularTotal.color }">
            <td>Total</td>
            <td>$ {{ calcularTotal.valor }}</td>
          </tr>
        </table>
      </div>
      <h3 v-else class="alert alert-info">No hay gastos ingresados</h3>
    </div>
  </section>
</template>

<script>
export default {
  name: "src-componentes-ingreso",
  props: [],
  mounted() {},
  data() {
    return {
      formstate: {},
      formData: this.getInitialData(),
      gastos: [],
      gastoTotal: 0,
      presupuesto: 0,
    };
  },
  methods: {
    getInitialData() {
      return {
        nombre: null,
        descripcion: null,
        importe: null,
        fecha: null,
      };
    },
    enviar() {
      let gasto = { ...this.formData };
      gasto.fecha = new Date().toLocaleString();

      console.log(gasto);

      this.gastos.push(gasto);
      this.gastoTotal += gasto.importe;
      this.formData = this.getInitialData();
      this.formstate._reset();
    },
  },
  computed: {
    calcularTotal() {
      let total = this.gastoTotal;
      let color;
      if (total < 1000) color = "green";
      if (total >= 1000 && total <= 5000) color = "magenta";
      if (total > 5000) color = "orange";

      return { valor: total, color };
    },
    getColorPresupest() {
      return this.presupuesto > this.gastoTotal ? "red" : "";
    },
  },
};
</script>

<style scoped lang="css">
.src-componentes-ingreso {
}
</style>
