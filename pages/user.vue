<template>
  <v-container class="justify-center mt-5">
    <TabelaDados v-if="items.length > 0" @editItem="editItem" @abrirDialog="ativo = true" titulo="users" :loading="loading" @deleteItem="deleteItem" :headers="headers" :items="items"></TabelaDados>
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
              v-model="users.id"
            >
            </v-text-field>
          </v-col>
          <v-col>
            <v-text-field
              variant="outlined"
              label="username"
              placeholder="username"
              v-model="users.username"
            >
            </v-text-field>
          </v-col>
        </v-row>
      <v-card-text>
        <v-col>
            <v-text-field
            variant="outlined"
            label="cpf"
            placeholder="cpf"
            v-model="users.cpf"
           >
           </v-text-field>
         </v-col>
       </v-card-text>
       <v-row>
          <v-col>
            <v-text-field
              variant="outlined"
              label="name"
              placeholder="name"
              v-model="users.name"
            >
            </v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              variant="outlined"
              label="phone"
              placeholder="phone"
              v-model="users.phone"
            >
            </v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              variant="outlined"
              label="passwordHash"
              placeholder="passwordHash"
              v-model="users.passwordHash"
            >
            </v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              variant="outlined"
              label="role"
              placeholder="role"
              v-model="users.role"
            >
            </v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              variant="outlined"
              label="cart"
              placeholder="cart"
              v-model="users.cart"
            >
            </v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              variant="outlined"
              label="email"
              placeholder="email"
              v-model="users.email"
            >
            </v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              variant="outlined"
              label="recuperation"
              placeholder="recuperation"
              v-model="users.recuperation"
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
      users: {
        id: null,
        username: null,
        cpf: null,
        name: null,
        phone: null,
        passwordHash: null,
        role: null,
        cart: null,
        email: null,
        recuperation: null,                        
      },
      headers: [
        {
          title: 'Identificador',
          key: 'id'
        },
        {
          title: 'username',
          key: 'username'
        },
        {
          title: 'cpf',
          key: 'cpf'
        },
        {
          title: 'name',
          key: 'name'
        },
        {
          title: 'phone',
          key: 'phone'
        },
        {
          title: 'passwordHash',
          key: 'passwordHash'
        },
        {
          title: 'role',
          key: 'role'
        },
        {
          title: 'cart',
          key:   'cart'
        },
        {
          title: 'email',
          key: 'email'
        },
        {
          title: 'recuperation',
          key: 'recuperation'
        },
        {
          title: 'token',
          key: 'token'
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
      return this.users.id ? 'Editar': 'Criar';
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
      this.users = {
        id: null,
        username: null,
        cpf: null,
        name: null,
        phone: null,
        passwordHash: null,
        token: null,
        role: null,
        cart: null,
        email: null,
        recuperation: null,
      }
      this.ativo = false;
    },

    async persist() {
      if (this.address.id) {
        const response = await this.$api.post(`/users/persist/${this.users.id}`, this.users);
      } else {
        const response = await this.$api.post('/users/persist', this.users);
      }
      this.resetAtividade()
      await this.getItems();
    },

    editItem(item) {
      this.users = {
        ...item
      };
      this.ativo = true;
    },

    async deleteItem(item) {
      if (confirm(`Deseja deletar o registro com id ${item.id}`)) {
        const response = await this.$api.post('/users/destroy'  );
        if (response.type == 'error') {
          alert(response.message);
        }
      }
      await this.getItems();
    },

    async getItems() {
      const response = await this.$api.get('/users');
      this.items = response.data;
      this.loading = false;
    }
  }
}
</script>