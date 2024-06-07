<template>
  <v-container class="justify-center mt-5">
    <TabelaDados v-if="items.length > 0" @editItem="editItem" @abrirDialog="ativo = true" titulo="orders" :loading="loading" @deleteItem="deleteItem" :headers="headers" :items="items"></TabelaDados>
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
              v-model="orders.id"
            >
            </v-text-field>
          </v-col>
          <v-col>
            <v-text-field
              variant="outlined"
              label="status"
              placeholder="status"
              v-model="orders.status"
            >
            </v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              variant="outlined"
              label="totalDiscount"
              placeholder="totalDiscount"
              v-model="orders.totalDiscount"
            >
            </v-text-field>
          </v-col>
          <v-col>
            <v-text-field
              variant="outlined"
              label="Cupom"
              placeholder="Cupom"
              v-model="orders.cupomId"
            >
            </v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              variant="outlined"
              label="ID Endereço"
              placeholder="ID Endereço"
              v-model="orders.idAddress"
            >
            </v-text-field>
          </v-col>
          <v-col>
            <v-text-field
              variant="outlined"
              label="ID de Pagamento"
              placeholder="ID de Pagamento"
              v-model="orders.paymentId"
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
      orders: {
        id: null,
        status: null,
        totalDiscount: null,
      },
      headers: [
        {
          title: 'Identificador',
          key: 'id'
        },
        {
          title: 'status',
          key: 'status'
        },
        {
          title: 'Desconto',
          key: 'totalDiscount'
        },
        {
          title: 'ID Cupom',
          key: 'cupomId'
        },
        {
          title: 'Id Endereço',
          key: 'idAddress'
        },
        {
          title: 'ID Pagamento',
          key: 'paymentId'
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
      return this.orders.id ? 'Editar': 'Criar';
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
      this.orders = {
        id: null,
        status: null,
        totalDiscount: null,
      }
      this.ativo = false;
    },

    async persist() {
      if (this.orders.id) {
        const response = await this.$api.post(`/orders/persist/${this.orders.id}`, this.orders);
      } else {
        const response = await this.$api.post('/orders/persist', this.orders);
      }
      this.resetAtividade()
      await this.getItems();
    },

    editItem(item) {
      this.orders = {
        ...item
      };
      this.ativo = true;
    },

    async deleteItem(item) {
      if (confirm(`Deseja deletar o registro com id ${item.id}`)) {
        const response = await this.$api.post('/orders/destroy'  );
        if (response.type == 'error') {
          alert(response.message);
        }
      }
      await this.getItems();
    },

    async getItems() {
      const response = await this.$api.get('/orders');
      this.items = response.data;
      this.loading = false;
    }
  }
}
</script>