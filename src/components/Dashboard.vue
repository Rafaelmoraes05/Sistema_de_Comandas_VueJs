<template>
  <div id="mae">
    <table id="burger-table" v-if="burgers">
      <thead>
        <tr id="burger-table-heading">
          <th class="order-id">Nº Pedido</th>
          <th>Cliente</th>
          <th>Pão</th>
          <th>Carne</th>
          <th>Opcionais</th>
          <th>Ações</th>
        </tr>
      </thead>
      <tbody id="burger-table-rows">
        <tr class="burger-table-row" v-for="burger in burgers" :key="burger.id">
          <td class="order-number">{{ burger.id }}</td>
          <td>{{ burger.nome }}</td>
          <td>{{ burger.pao }}</td>
          <td>{{ burger.carne }}</td>
          <td>
            <ul>
              <li v-for="(opcional, index) in burger.opcionais" :key="index">{{ opcional }}</li>
            </ul>
          </td>
          <td>
            <select name="status" class="status" @change="updateBurger($event, burger.id)">
              <option :value="s.tipo" v-for="s in status" :key="s.id" :selected="burger.status == s.tipo">
                {{ s.tipo }}
              </option>
            </select>
            <button class="delete-btn" @click="deleteBurger(burger.id, burger.nome)">Cancelar</button>
          </td>
        </tr>
      </tbody>
    </table>
    <div v-else>
      <h2>Não há pedidos no momento!</h2>
    </div>
  </div>
</template>

<script>
export default {
  name: "Dashboard",
  data() {
    return {
      burgers: null,
      burger_id: null,
      status: []
    }
  },
  methods: {
    async getPedidos() {
      const req = await fetch('https://jsonservercomandas.onrender.com/burgers')

      const data = await req.json()

      this.burgers = data

      // Resgata os status de pedidos
      this.getStatus()
    },
    async getStatus() {
      const req = await fetch('https://jsonservercomandas.onrender.com/status')

      const data = await req.json()

      this.status = data
    },
    async deleteBurger(id, nome_cliente) {
      const req = await fetch(`https://jsonservercomandas.onrender.com/burgers/${id}`, {
        method: "DELETE"
      });

      const res = await req.json()

      this.getPedidos()

      alert(`O pedido de ${nome_cliente} foi deletado com sucesso!`)
    },
    async updateBurger(event, id) {
      const option = event.target.value;

      const dataJson = JSON.stringify({ status: option });

      const req = await fetch(`https://jsonservercomandas.onrender.com/burgers/${id}`, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: dataJson
      });

      const res = await req.json()

      alert(`O status do pedido de ${res.nome} foi alterado com sucesso!`)
    }
  },
  mounted() {
    this.getPedidos()
  }
}
</script>

<style scoped>
#mae {
  overflow-x: scroll;
  max-width: 95%;
  margin: 0 0 0 5%;
}

table {
  width: 100%;
  border-collapse: collapse;
}

thead {
  background-color: #f2f2f2;
}

th, td {
  padding: 12px;
  border: 1px solid #ddd;
  text-align: left;
}

.order-id, .order-number {
  width: 5%;
}

th, .order-number, td {
  width: 19%;
}

td ul {
  padding: 0;
  margin: 0;
  list-style: none;
}

select {
  padding: 12px 6px;
  margin-right: 12px;
}

.delete-btn {
  background-color: #222;
  color: #FCBA03;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: .5s;
}

.delete-btn:hover {
  background-color: transparent;
  color: #222;
}

/* Media Queries */
@media (max-width: 768px) {
  #burger-table-container {
    overflow-x: auto; /* Permite rolagem horizontal em dispositivos móveis */
  }

  th, td {
    min-width: 120px; /* Ajusta a largura mínima das colunas para dispositivos menores */
  }

  select {
    padding: 10px 5px;
    margin-right: 10px;
  }

  .delete-btn {
    padding: 8px;
    font-size: 14px;
  }
}

@media (max-width: 480px) {
  #burger-table-container {
    overflow-x: auto; /* Permite rolagem horizontal em dispositivos móveis */
  }

  th, td {
    min-width: 100px; /* Ajusta a largura mínima das colunas para dispositivos menores */
  }

  select {
    padding: 8px 4px;
    margin-right: 8px;
  }

  .delete-btn {
    padding: 6px;
    font-size: 12px;
  }
}
</style>