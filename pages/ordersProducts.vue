<template>
  <v-container class="justify-center mt-5">
    <TabelaDados v-if="items.length > 0" @editItem="editItem" @abrirDialog="ativo = true" titulo="ordersProducts" :loading="loading" @deleteItem="deleteItem" :headers="headers" :items="items"></TabelaDados>
    <h1 v-else> Sem items</h1>
  </v-container>
  <v-dialog
      v-model="ativo"
      max-width="500"
    >
    <v-card
      height="600"
      width="700"
    >
      <v-card-title>
        <h1>
          {{ tituloDialog }} um Evento
        </h1>
      </v-card-title>
      <v-card-text>
        <v-row>
          <v-col>
            <v-text-field
              variant="outlined"
              label="Id"
              placeholder="Identificador"
              disabled
              v-model="ordersProducts.id"
            >
            </v-text-field>
          </v-col>
          <v-col>
            <v-text-field
              variant="outlined"
              label="priceProducts"
              placeholder="priceProducts"
              v-model="ordersProducts.priceProducts"
            >
            </v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              variant="outlined"
              label="quantity"
              placeholder="quantity"
              v-model="ordersProducts.quantity"
            >
            </v-text-field>
          </v-col>
        </v-row>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn
          color="green"
          variant="outlined"
          @click="persist"
        >
          Salvar
        </v-btn>
      </v-card-actions>
    </v-card>
    </v-dialog>
</template>

<script>
export default {
  data: () => {
    return {
      dialog: false,
      valor: 0,
      ativo: false,
      loading: true,
      textoUsuario: null,
      ordersProducts: {
        id: null,
        priceProducts: null,
        quantity: null,
      },
      headers: [
        {
          title: 'Identificador',
          key: 'id'
        },
        {
          title: 'priceProducts',
          key: 'priceProducts'
        },
        {
          title: 'quantity',
          key: 'quantity'
        },
        {
          title: 'Actions',
          key: 'actions',
          sortable: false
        },
      ],
      items: [],
    }
  },

  async created(){
    await this.getItems();
  },

  computed: {
    tituloDialog: function() {
      return this.ordersProducts.id ? 'Editar': 'Criar';
    }
  },

  watch: {
    ativo(valor) {
      if (valor == false) {
        this.resetAtividade()
      }
    }
  },

  methods: {
    resetAtividade() {
      this.ordersProducts = {
        id: null,
        priceProducts: null,
        quantity: null,
      }
      this.ativo = false;
    },

    async persist() {
      if (this.ordersProducts.id) {
        const response = await this.$api.post(`/ordersProducts/persist/${this.ordersProducts.id}`, this.ordersProducts);
      } else {
        const response = await this.$api.post('/ordersProducts/persist', this.ordersProducts);
      }
      this.resetAtividade()
      await this.getItems();
    },

    editItem(item) {
      this.ordersProducts = {
        ...item
      };
      this.ativo = true;
    },

    async deleteItem(item) {
      if (confirm(`Deseja deletar o registro com id ${item.id}`)) {
        const response = await this.$api.post('/ordersProducts/destroy'  );
        if (response.type == 'error') {
          alert(response.message);
        }
      }
      await this.getItems();
    },

    async getItems() {
      const response = await this.$api.get('/ordersProducts');
      this.items = response.data;
      this.loading = false;
    }
  }
}
</script>