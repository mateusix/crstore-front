<template>
  <v-container class="justify-center mt-5">
    <TabelaDados v-if="items.length > 0" @editItem="editItem" @abrirDialog="ativo = true" titulo="address" :loading="loading" @deleteItem="deleteItem" :headers="headers" :items="items"></TabelaDados>
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
              v-model="address.id"
            >
            </v-text-field>
          </v-col>
          <v-col>
            <v-text-field
              variant="outlined"
              label="zipcode"
              placeholder="zipcode"
              v-model="address.zipCode"
            >
            </v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              variant="outlined"
              label="state"
              placeholder="state"
              v-model="address.state"
            >
            </v-text-field>
          </v-col>
          <v-col>
            <v-text-field
              variant="outlined"
              label="city"
              placeholder="city"
              v-model="address.city"
            >
            </v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              variant="outlined"
              label="street"
              placeholder="street"
              v-model="address.street"
            >
            </v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              variant="outlined"
              label="district"
              placeholder="district"
              v-model="address.district"
            >
            </v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              variant="outlined"
              label="number"
              placeholder="number"
              v-model="address.numberForget"
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
      address: {
        id: null,
        zipCode: null,
        state: null,
        city: null,
        street: null,
        district: null,
        numberForget: null
      },
      headers: [
        {
          title: 'Identificador',
          key: 'id'
        },
        {
          title: 'zipCode',
          key: 'zipCode'
        },
        {
          title: 'state',
          key: 'state'
        },
        {
          title: 'city',
          key: 'city'
        },
        {
          title: 'street',
          key: 'street'
        },
        {
          title: 'district',
          key: 'district'
        },
        {
          title: 'numberForget',
          key: 'numberForget'
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
      return this.address.id ? 'Editar': 'Criar';
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
      this.address = {
        id: null,
        zipcode: null,
        state: null,
        city: null,
        street: null,
        district: null,
        numberForget: null,
      }
      this.ativo = false;
    },

    async persist() {
      if (this.address.id) {
        const response = await this.$api.post(`/address/persist/${this.address.id}`, this.address);
      } else {
        const response = await this.$api.post('/address/persist', this.address);
      }
      this.resetAtividade()
      await this.getItems();
    },

    editItem(item) {
      this.address = {
        ...item
      };
      this.ativo = true;
    },

    async deleteItem(item) {
      if (confirm(`Deseja deletar o registro com id ${item.id}`)) {
        const response = await this.$api.post('/address/destroy'  );
        if (response.type == 'error') {
          alert(response.message);
        }
      }
      await this.getItems();
    },

    async getItems() {
      const response = await this.$api.get('/address');
      this.items = response.data;
      this.loading = false;
    }
  }
}
</script>