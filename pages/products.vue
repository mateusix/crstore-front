<template>
  <v-container class="justify-center mt-5">
    <TabelaDados v-if="items.length > 0" @editItem="editItem" @abrirDialog="ativo = true" titulo="products" :loading="loading" @deleteItem="deleteItem" :headers="headers" :items="items"></TabelaDados>
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
              v-model="products.id"
            >
            </v-text-field>
          </v-col>
          <v-col>
            <v-text-field
              variant="outlined"
              label="name"
              placeholder="name"
              v-model="products.name"
            >
            </v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              variant="outlined"
              label="price"
              placeholder="price"
              v-model="products.price"
            >
            </v-text-field>
          </v-col>
          <v-col>
            <v-text-field
              variant="outlined"
              label="image"
              placeholder="image"
              v-model="products.image"
            >
            </v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              variant="outlined"
              label="description"
              placeholder="description"
              v-model="products.description"
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
      products: {
        id: null,
        name: null,
        price: null,
        image: null,
        description: null,        
      },
      headers: [
        {
          title: 'Identificador',
          key: 'id'
        },
        {
          title: 'name',
          key: 'name'
        },
        {
          title: 'price',
          key: 'price'
        },
        {
          title: 'image',
          key: 'image'
        },
        {
          title: 'description',
          key: 'description'
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
      return this.products.id ? 'Editar': 'Criar';
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
      this.products = {
        id: null,
        name: null,
        price: null,
        image: null,
        description: null,        
      }
      this.ativo = false;
    },

    async persist() {
      if (this.products.id) {
        const response = await this.$api.post(`/products/persist/${this.products.id}`, this.products);
      } else {
        const response = await this.$api.post('/products/persist', this.products);
      }
      this.resetAtividade()
      await this.getItems();
    },

    editItem(item) {
      this.products = {
        ...item
      };
      this.ativo = true;
    },

    async deleteItem(item) {
      if (confirm(`Deseja deletar o registro com id ${item.id}`)) {
        const response = await this.$api.post('/products/destroy'  );
        if (response.type == 'error') {
          alert(response.message);
        }
      }
      await this.getItems();
    },

    async getItems() {
      const response = await this.$api.get('/products');
      this.items = response.data;
      this.loading = false;
      console.log(this.items)
    }
  }
}
</script>