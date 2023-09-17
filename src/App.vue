<template>
  <v-layout class="rounded rounded-md">
    <!-- Barra de aplicación -->
    <v-app-bar color="grey-lighten-2" title=""></v-app-bar>

    <!-- Contenido principal -->
    <v-main class="d-flex align-center justify-center" style="min-height: 300px">
      <!-- Botón para abrir el diálogo -->
      <v-btn color="grey-lighten-2" @click="showDialog">Iniciar formulario</v-btn>

      <!-- Diálogo principal -->
      <v-dialog v-model="dialog" max-width="500px">
        <v-card>
          <v-card-title>Formulario</v-card-title>
          <v-card-text>
            <!-- Formulario -->
            <v-form ref="form">
              <!-- Paso 1 -->
              <v-card v-if="box1">
                <v-text-field
                  v-model="name"
                  :counter="10"
                  :rules="nameRules"
                  label="Name"
                  required
                ></v-text-field>
                <v-text-field
                  v-model="description"
                  :counter="10"
                  label="Description (optional)"
                ></v-text-field>
                <v-text-field
                  v-model="date"
                  :rules="nameRules"
                  label="Date"
                  type="date"
                ></v-text-field>
                <v-switch v-model="showMessages" label="Public | Private"></v-switch>
                <div class="d-flex flex-column">
                  <v-btn color="success" class="mt-4" block @click="changeTo2">
                    Validate
                  </v-btn>
                  <v-btn color="error" class="mt-4" block @click="reset">
                    Reset Form
                  </v-btn>
                  <v-btn color="warning" class="mt-4" block @click="closeDialog">
                    Close
                  </v-btn>
                </div>
              </v-card>

              <!-- Paso 2 -->
              <v-card v-if="box2">
                <v-text-field
                  v-model="enter"
                  :counter="10"
                  label="Enter"
                  required
                ></v-text-field>
                <v-select
                  v-model="selectedType"
                  label="Select"
                  :items="['type 1', 'type 2', 'type 3', 'type 4']"
                ></v-select>
                <v-row>
                  <v-col cols="12">
                    <v-text-field
                      v-model="searchText"
                      label="Buscar"
                      outlined
                      @input="filterCheckboxes"
                    ></v-text-field>
                  </v-col>
                </v-row>

                <!-- Agrega un div con v-scrollbar para la lista de checkboxes -->
                <div class="scrollable-list">
                  <v-scrollbar>
                    <ul>
                      <li v-for="(item, index) in filteredItems" :key="index">
                        <v-checkbox v-model="item.selected">
                          <!-- Iconos condicionales -->
                          <template v-slot:label>
                            <v-icon class="mr-2" size="24">{{ item.icon }}</v-icon>
                            {{ item.label }}
                          </template>
                        </v-checkbox>
                      </li>
                    </ul>
                  </v-scrollbar>
                </div>

                <div class="d-flex flex-column">
                  <v-btn color="success" class="mt-4" block @click="changeTo3">
                    Validate
                  </v-btn>
                  <v-btn color="error" class="mt-4" block @click="reset">
                    Reset Form
                  </v-btn>
                  <v-btn color="warning" class="mt-4" block @click="closeDialog">
                    Close
                  </v-btn>
                </div>
              </v-card>

              <!-- Paso 3 -->
              <v-card v-if="box3">
                <v-radio-group
                  v-model="selectedRadio"
                  inline
                  label="Radio button options"
                >
                  <v-radio label="Option 1" value="1"></v-radio>
                  <v-radio label="Option 2" value="2"></v-radio>
                  <v-radio label="Option 3" value="3"></v-radio>
                </v-radio-group>
                <v-row label="Check-box options">
                  <v-col cols="3">
                    <v-checkbox v-model="checkbox1" label="Option 1"></v-checkbox>
                  </v-col>
                  <v-col cols="3">
                    <v-checkbox v-model="checkbox2" label="Option 2"></v-checkbox>
                  </v-col>
                </v-row>
                <div class="d-flex flex-column">
                  <v-btn color="success" class="mt-4" block @click="submitForm">Submit</v-btn>
                  <v-btn color="error" class="mt-4" block @click="reset">
                    Reset Form
                  </v-btn>
                  <v-btn color="warning" class="mt-4" block @click="closeDialog">
                    Close
                  </v-btn>
                </div>
              </v-card>
            </v-form>
          </v-card-text>
        </v-card>
      </v-dialog>
    </v-main>
  </v-layout>
</template>

<script>
export default {
  data() {
    return {
      // Datos del formulario
      dialog: false,
      box1: true,
      box2: false,
      box3: false,
      name: "",
      description: "",
      date: "",
      showMessages: false,
      enter: "",
      selectedType: null,
      items: [
        { label: "Instagram Download 1", selected: false },
        { label: "Instagram Download 2", selected: false },
        { label: "Facebook test", selected: false },
        { label: "Twitter test", selected: false },
        { label: "Twiter download 1", selected: false },
      ],
      searchText: "",
      selectedRadio: null,
      checkbox1: false,
      checkbox2: false,
    };
  },
  computed: {
    nameRules() {
      return [
        (v) => !!v || "Name is required",
        (v) => (v && v.length <= 10) || "Name must be less than 10 characters",
      ];
    },
    filteredItems() {
      const searchText = this.searchText.toLowerCase();
      return this.items
        .filter((item) =>
          item.label.toLowerCase().includes(searchText)
        )
        .map((item) => {
          if (item.label.toLowerCase().includes("instagram")) {
            item.icon = "mdi-instagram";
          } else if (item.label.toLowerCase().includes("facebook")) {
            item.icon = "mdi-facebook";
          } else {
            item.icon = "";
          }
          return item;
        });
    },
  },
  methods: {
    showDialog() {
      this.dialog = true;
    },
    closeDialog() {
      this.dialog = false;
      this.box2 = false;
      this.box3 = false;
      this.box1 = true;
    },
    changeTo2() {
      this.box1 = false;
      this.box2 = true;
    },
    changeTo3() {
      this.box2 = false;
      this.box3 = true;
    },
    reset() {
      this.box2 = false;
      this.box3 = false;
      this.box1 = true;
      this.name = "";
      this.description = "";
      this.date = "";
      this.showMessages = false;
      this.enter = "";
      this.selectedType = null;
      this.searchText = "";
      this.selectedRadio = null;
      this.checkbox1 = false;
      this.checkbox2 = false;
      this.items.forEach((item) => {
        item.selected = false;
      });
    },
    filterCheckboxes() {
      const searchText = this.searchText.toLowerCase();
      this.filteredItems = this.items
        .filter((item) =>
          item.label.toLowerCase().includes(searchText)
        )
        .map((item) => {
          if (item.label.toLowerCase().includes("instagram")) {
            item.icon = "mdi-instagram";
          } else if (item.label.toLowerCase().includes("facebook")) {
            item.icon = "mdi-facebook";
          } else {
            item.icon = "";
          }
          return item;
        });
    },
    submitForm() {
      // Lógica para enviar el formulario, puedes agregarla aquí
      this.dialog = false;
    },
  },
};
</script>

<style>
.scrollable-list {
  max-height: 220px;
  overflow-y: auto;
}
</style>
