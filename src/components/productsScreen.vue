<template>
  <div id="productsScreen" v-if="paymentScreen == false && loginScreen == false">
    <div v-for="product in products" :key="product.id" class="product">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        height="24px"
        viewBox="0 -960 960 960"
        width="24px"
        fill="aaa"
      >
        <path
          d="M200-80q-33 0-56.5-23.5T120-160v-451q-18-11-29-28.5T80-680v-120q0-33 23.5-56.5T160-880h640q33 0 56.5 23.5T880-800v120q0 23-11 40.5T840-611v451q0 33-23.5 56.5T760-80H200Zm0-520v440h560v-440H200Zm-40-80h640v-120H160v120Zm200 280h240v-80H360v80Zm120 20Z"
        />
      </svg>
      <img :src="product.image" :alt="product.name" />
      <h1>{{ product.name }}</h1>
      <p>{{ product.price }}</p>
      <button @click="add(product.id)">+</button>
      <h2>{{ product.quantity }}</h2>
      <button @click="remove(product.id)">-</button>
    </div>

    <div v-if="selectedProducts.length > 0" class="selectedProducts">
      <svg
        id="cart"
        xmlns="http://www.w3.org/2000/svg"
        height="24px"
        viewBox="0 -960 960 960"
        width="24px"
        fill="aaa"
      >
        <path
          d="m480-560-56-56 63-64H320v-80h167l-64-64 57-56 160 160-160 160ZM280-80q-33 0-56.5-23.5T200-160q0-33 23.5-56.5T280-240q33 0 56.5 23.5T360-160q0 33-23.5 56.5T280-80Zm400 0q-33 0-56.5-23.5T600-160q0-33 23.5-56.5T680-240q33 0 56.5 23.5T760-160q0 33-23.5 56.5T680-80ZM40-800v-80h131l170 360h280l156-280h91L692-482q-11 20-29.5 31T622-440H324l-44 80h480v80H280q-45 0-68.5-39t-1.5-79l54-98-144-304H40Z"
        />
      </svg>
      <div class="list" v-for="product in selectedProducts" :key="product.id">
        <button @click="add(product.id)">+</button>
        <p id="description">
          {{ product.quantity }}
          {{ product.name }}
        </p>
        <button @click="remove(product.id)">-</button>
      </div>

      <div id="total">
        <h1>Total</h1>
        <p>R${{ varTotal.toFixed(2) }}</p>

        <div id="buttons">
          <button id="removeAllButton" @click="removeAll()">
            <svg
              id="delete"
              xmlns="http://www.w3.org/2000/svg"
              height="24px"
              viewBox="0 -960 960 960"
              width="24px"
              fill="#fff"
            >
              <path
                d="M280-120q-33 0-56.5-23.5T200-200v-520h-40v-80h200v-40h240v40h200v80h-40v520q0 33-23.5 56.5T680-120H280Zm400-600H280v520h400v-520ZM360-280h80v-360h-80v360Zm160 0h80v-360h-80v360ZM280-720v520-520Z"
              />
            </svg>
          </button>
          <button @click="paymentScreen = true" id="paymentButton">
            <h2>Pagamento</h2>
          </button>
        </div>
      </div>
    </div>
  </div>

  <div id="paymentScreen" v-if="paymentScreen == true">
    <button @click="paymentScreen = false">
      <svg
        id="return"
        xmlns="http://www.w3.org/2000/svg"
        height="24px"
        viewBox="0 -960 960 960"
        width="24px"
        fill="#aa"
      >
        <path d="m313-440 224 224-57 56-320-320 320-320 57 56-224 224h487v80H313Z" />
      </svg>
    </button>
    <h1>Pedido</h1>
    <ul>
      <li v-for="product in selectedProducts" :key="product.id">
        {{ product.name }} - Qtd: {{ product.quantity }} - R${{ product.numberPrice.toFixed(2) }}
      </li>
    </ul>

    <h2>Total</h2>

    <p>R${{ this.varTotal.toFixed(2) }}</p>
  </div>

  <div id="loginScreen">
    <div id="iconLogin" @click="loginScreen = true" v-if="loginScreen == false">
      <svg
        id="svgLogin"
        xmlns="http://www.w3.org/2000/svg"
        height="24px"
        viewBox="0 -960 960 960"
        width="24px"
        fill="#1f1f1f"
      >
        <path
          d="M234-276q51-39 114-61.5T480-360q69 0 132 22.5T726-276q35-41 54.5-93T800-480q0-133-93.5-226.5T480-800q-133 0-226.5 93.5T160-480q0 59 19.5 111t54.5 93Zm246-164q-59 0-99.5-40.5T340-580q0-59 40.5-99.5T480-720q59 0 99.5 40.5T620-580q0 59-40.5 99.5T480-440Zm0 360q-83 0-156-31.5T197-197q-54-54-85.5-127T80-480q0-83 31.5-156T197-763q54-54 127-85.5T480-880q83 0 156 31.5T763-763q54 54 85.5 127T880-480q0 83-31.5 156T763-197q-54 54-127 85.5T480-80Zm0-80q53 0 100-15.5t86-44.5q-39-29-86-44.5T480-280q-53 0-100 15.5T294-220q39 29 86 44.5T480-160Zm0-360q26 0 43-17t17-43q0-26-17-43t-43-17q-26 0-43 17t-17 43q0 26 17 43t43 17Zm0-60Zm0 360Z"
        />
      </svg>
    </div>
    <div id="loginForm" v-if="loginScreen == true">
      <button @click="loginScreen = false">
        <svg
          id="return"
          xmlns="http://www.w3.org/2000/svg"
          height="24px"
          viewBox="0 -960 960 960"
          width="24px"
          fill="#aa"
        >
          <path d="m313-440 224 224-57 56-320-320 320-320 57 56-224 224h487v80H313Z" />
        </svg>
      </button>
      <form method="post">
        <h1>Login</h1>

        <label for="emailID">E-mail: </label>
        <input
          type="text"
          id="emailID"
          placeholder="seuemail@gmail.com"
          v-model="this.loginUser.email"
        />

        <label for="passwordID">Senha: </label>
        <input
          type="password"
          id="passwordID"
          v-model="this.loginUser.password"
          placeholder="***********"
        />

        <button id="loginButton">Cadastrar</button>
      </form>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'

export default {
  name: 'productsScreen',
  data() {
    return {
      products: ref([
        {
          id: 0,
          name: 'BigMac',
          numberPrice: 10.0,
          price: 'R$10,00',
          image: 'src/assets/bigmac.png',
          quantity: 0,
          selected: false,
        },
        {
          id: 1,
          name: 'MacLancheFeliz',
          price: 'R$20,00',
          numberPrice: 20,
          image: 'src/assets/maclanchefeliz.png',
          quantity: 0,
          selected: false,
        },
        {
          id: 2,
          name: 'Duplo Quarterão',
          price: 'R$13,00',
          numberPrice: 13,
          image: 'src/assets/duploquarterao.png',
          quantity: 0,
          selected: false,
        },
        {
          id: 3,
          name: 'Duplo Bacon',
          price: 'R$15,00',
          numberPrice: 15,
          image: 'src/assets/duplo-bacon.png',
          quantity: 0,
          selected: false,
        },
        {
          id: 4,
          name: 'Nugget',
          price: 'R$7,00',
          numberPrice: 7,
          image: 'src/assets/nugget.png',
          quantity: 0,
          selected: false,
        },
        {
          id: 5,
          name: 'Batata Frita',
          price: 'R$6,50',
          numberPrice: 6.5,
          image: 'src/assets/batata-frita.png',
          quantity: 0,
          selected: false,
        },
        {
          id: 6,
          name: 'Coca-Cola',
          price: 'R$3,50',
          numberPrice: 3.5,
          image: 'src/assets/coca-cola.png',
          quantity: 0,
          selected: false,
        },
        {
          id: 7,
          name: 'Sprite',
          price: 'R$3,50',
          numberPrice: 3.5,
          image: 'src/assets/sprite.png',
          quantity: 0,
          selected: false,
        },
      ]),
      users: ref([
        {
          email: undefined,
          password: undefined,
        },
      ]),
      loginUser: {
        email: undefined,
        password: undefined,
      },
      varTotal: 0,
      paymentScreen: false,
      loginScreen: false,
    }
  },
  methods: {
    add(index) {
      this.products[index].quantity++
      this.products[index].selected = true
      this.varTotal = this.varTotal + this.products[index].numberPrice
    },
    remove(index) {
      if (this.products[index].quantity > 0) {
        this.products[index].quantity--
        this.varTotal = this.varTotal - this.products[index].numberPrice
      }
      if (this.products[index].quantity == 0) {
        this.products[index].selected = false
      }
    },
    removeAll() {
      for (var index = 0; index <= this.products.length; index++) {
        if (this.products[index].selected) {
          this.products[index].selected = false
          this.products[index].quantity = 0
          this.varTotal = 0
        }
      }
    },
  },
  computed: {
    selectedProducts() {
      return this.products.filter((product) => product.selected)
    },
  },
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=DM+Sans:ital,opsz,wght@0,9..40,100..1000;1,9..40,100..1000&display=swap');

@media screen and (max-width: 768px) {
  svg {
    display: none;
  }
  svg#delete {
    display: inline;
  }
  svg#return {
    display: inline;
  }
}
* {
  font-family: 'DM Sans', sans-serif;
}
body {
  background-image: linear-gradient(to left, red, orange, yellow);
  /* background-image: url(../assets/ambient.jpg); */
  background-attachment: fixed;
  background-size: cover;
  background-repeat: no-repeat;
  background-position: 50%, 50%;
}
#productsScreen {
  position: absolute;
  top: 10px;
  left: 50%;
  transform: translateX(-50%);
  margin: auto;
  font-family: 'DM Sans', sans-serif;
  display: flex; /* Definindo o flexbox */
  flex-wrap: wrap; /* Permitindo que os itens quebrem para uma nova linha */
  gap: 16px; /* Espaçamento entre os itens */
  justify-content: center; /* Centraliza os itens horizontalmente */
  align-items: center;
  padding: 20px; /* Espaçamento interno */
  max-width: 600px; /* Largura máxima para o contêiner */
}
h2 {
  display: inline-block;
  margin-inline: 10px;
  justify-content: center;
  margin-top: 10px;
}
img {
  height: 150px;
  width: 200px;
}
.product {
  position: relative;
  display: flexbox;
  flex: 1 1 200px; /* Flexível, com um tamanho base de 200px */
  background-color: #ffffff; /* Cor de fundo */
  color: black; /* Cor do texto */
  padding: 20px; /* Espaçamento interno */
  text-align: center; /* Alinhamento do texto */
  border-radius: 8px; /* Bordas arredondadas */
  box-shadow: 0px 15px 30px rgb(58, 58, 58);
  border: 1px solid black;
}
.product svg {
  position: absolute;
  top: 10px;
  left: 10px;
}
.selectedProducts {
  position: relative;
  display: flexbox;
  flex: 1 1 200px; /* Flexível, com um tamanho base de 200px */
  background-color: #ffffff; /* Cor de fundo */
  color: black; /* Cor do texto */
  padding: 20px; /* Espaçamento interno */
  text-align: center; /* Alinhamento do texto */
  border-radius: 8px; /* Bordas arredondadas */
  box-shadow: 0px 15px 30px rgb(58, 58, 58);
  border: 1px solid black;
  min-height: 300px;
}
.selectedProducts svg#cart {
  position: absolute;
  top: 25px;
  left: 25px;
}
.list {
  padding-bottom: 20px;
}
button {
  cursor: pointer;
  border-radius: 5px;
  background-color: #fff;
  width: 30px;
  height: 30px;
}
button:hover {
  background-color: black;
  border: 1px solid white;
  color: white;
  transition-duration: 0.3s;
}
#total {
  background-color: black;
  color: white;
  border-radius: 12px;
  padding: 10px;
}
#total p {
  padding-top: 10px;
  border-top: 1px solid white;
}
#total #buttons {
  display: flex; /* Definindo o flexbox */
  flex-wrap: wrap; /* Permitindo que os itens quebrem para uma nova linha */
  gap: 16px; /* Espaçamento entre os itens */
  justify-content: center; /* Centraliza os itens horizontalmente */
  align-items: center;
  padding: 20px; /* Espaçamento interno */
  max-width: 600px; /* Largura máxima para o contêiner */
}
#total #buttons button {
  flex: 1 1 200px; /* Flexível, com um tamanho base de 200px */
  color: black; /* Cor do texto */
  align-items: center;
  justify-content: center;
  text-align: center; /* Alinhamento do texto */
  border-radius: 8px; /* Bordas arredondadas */
}
#total #removeAllButton {
  display: inline-block;
  text-align: center;
  height: 40px;
  width: 40px;
  margin-top: 15px;
  justify-content: center;
  border: 1px solid black;
  background-color: rgb(255, 0, 0);
}
#total #removeAllButton:hover {
  border-radius: 10px;
  background-color: rgb(255, 140, 140);
  color: black;
  transition-duration: 0.2s;
}
.list #description {
  justify-content: center;
  text-align: center;
  display: inline-block;
  width: 125px;
}
#paymentButton {
  position: relative;
  display: inline-block;
  text-align: center;
  height: 40px;
  width: 40px;
  margin-top: 15px;
  justify-content: center;
  border: 1px solid black;
  background-color: rgb(35, 193, 0);
}
#paymentButton:hover {
  border-radius: 10px;
  background-color: rgb(35, 193, 140);
  transition-duration: 0.2s;
}
#paymentButton svg {
  position: absolute;
  left: 30px;
}
#paymentButton h2 {
  font-family: 'DM Sans';
  padding: 0;
  margin: 0;
  display: inline;
  color: white;
}
#paymentScreen {
  color: black;
  box-shadow: 0px 15px 30px rgb(58, 58, 58);
  font-family: 'DM Sans';
  border: 1px solid black;
  border-radius: 12px;
  padding: 30px;
  position: absolute;
  background-color: #fff;
  transform: translate(-50%, -50%);
  left: 50%;
  top: 50%;
}
#paymentScreen button {
  padding: 0px;
  position: absolute;
  left: 20px;
  top: 20px;
}
#paymentScreen button:hover {
  background-color: red;
  border: 1px solid black;
}
#paymentScreen h1 {
  text-align: center;
  margin-bottom: 20px;
}
#paymentScreen li {
  border-bottom: 1px solid black;
}
#paymentScreen h2 {
  display: block;
  text-align: center;
  margin-bottom: 15px;
}
#paymentScreen p {
  text-align: center;
}
#iconLogin {
  position: fixed;
  top: 30px;
  right: 20px;
  border: 1px solid black;
  background-color: #fff;
  padding: 20px;
  text-align: center;
  justify-content: center;
  border-radius: 10px;
  transition-duration: 0.2s;
  box-shadow: 0px 2px 25px black;
}
#iconLogin:hover {
  background-color: rgb(222, 222, 222);
  border-radius: 50%;
  transition-duration: 0.2s;
}
#svgLogin {
  display: block;
}
#loginForm {
  padding: 30px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
  padding: 30px;
  color: black;
  border: 1px solid black;
  border-radius: 12px;
  box-shadow: 0px 10px 20px black;
  font-size: 20px;
}
#loginForm form {
  padding: 30px;
}
#loginForm h1 {
  position: inherit;
  display: block;
  margin-left: 30%;
}
#loginForm label {
  display: block;
  padding-left: 0px;
  margin-top: 10px;
}
#loginForm input {
  border-radius: 12px;
  font-size: 20px;
  padding: 10px;
  display: block;
  border: 1px solid black;
}
#loginForm input:hover {
  border-radius: 8px;
  background-color: rgb(230, 230, 230);
  transition-duration: 0.2s;
}
#loginScreen button {
  height: 50px;
  width: 50px;
  position: absolute;
  left: 20px;
  top: 20px;
}
#loginScreen button:hover {
  background-color: red;
  border: 1px solid black;
}
#loginScreen #loginButton {
  transition-duration: 0.1s;
  width: 100%;
  position: inherit;
  display: block;
  margin-top: 20px;
  margin-inline: auto;
}
#loginScreen #loginButton:hover {
  background-color: rgb(0, 213, 0);
  color: white;
  transition-duration: 0.1s;
}
</style>
