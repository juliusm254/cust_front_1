<template>
  <div class="container">
    <div class="columns is-multiline">
      <div class="column is-12">
        <h1 class="title">Add Order</h1>
      </div>

      <div class="column is-12">
        <form @submit.prevent="submitForm">
          <div class="field">
            <label>Company</label>
            <div class="control">
              <input
                type="text"
                name="company"
                class="input"
                v-model="company"
              />
            </div>
          </div>

          <div class="field">
            <label>Truck</label>
            <div class="control">
              <!-- <select type="text" class="input" v-model="truck"> -->
              <select v-model="truck">
                <option disabled value="selected">Select Truck</option>
                <option
                  v-for="truck in trucks"
                  v-bind:key="truck"
                  :value="truck.id"
                >
                  {{ truck.registration }}
                </option>
              </select>
              <!-- <span style="padding-left:5%">Your Choice is: {{truck.registration}}</span> -->
            </div>
          </div>

          <div class="field">
            <label>Trailer</label>
            <div class="control">
              <!-- <select type="text" class="input" v-model="truck"> -->
              <select v-model="trailer">
                <option disabled value="selected">Select Truck</option>
                <option
                  v-for="trailer in trucks"
                  v-bind:key="trailer.id"
                  :value="trailer.id"
                >
                  {{ trailer.registration }}
                </option>
              </select>
              <!-- <span style="padding-left:5%">Your Choice is: {{trailer}}</span> -->
            </div>
          </div>

          <div class="field">
            <label>Driver</label>
            <div class="control">
              <!-- <select type="text" class="input" v-model="truck"> -->
              <select v-model="driver">
                <option disabled value="selected">Select Driver</option>
                <option
                  v-for="driver in drivers"
                  v-bind:key="driver.id"
                  :value="driver.id"
                >
                  {{ driver.name }}
                </option>
              </select>
              <!-- <span style="padding-left:5%">Your Choice is: {{driver.name}}</span> -->
            </div>
          </div>

          <div class="field">
            <label>Quantity</label>
            <div class="control">
              <input type="text" class="input" v-model="order_quantity" />
            </div>
          </div>

          <div class="field">
            <label>Destination</label>
            <div class="control">
              <input type="text" class="input" v-model="destination" />
            </div>
          </div>

          <div class="field">
            <div class="control">
              <button class="button is-success">Submit</button>
            </div>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

// import { toast } from 'bulma-toast'

export default {
  name: "Home",
  data() {
    return {
      trucks: [],
      drivers: [],
      company: "",
      driver: "",
      truck: "",
      order_quantity: "",
      trailer: "",
      destination: "",
    };
  },
  mounted() {
    this.getTrucks();
    this.getDrivers();
  },
  
      

  methods: {
    async getTrucks() {
      let config = {
        headers: {
          Authorization: "Bearer " + localStorage.getItem("access_token"),
          "Content-Type": "application/json",
        },
      };
      // this.$store.commit('setIsLoading', true)
      this.showNextButton = false;
      this.showPreviousButton = false;

      await axios
        .get(`/customer-truck/`, config)
        .then((response) => {
          console.log(response.data);
          this.trucks = response.data;
          // this.num_orders = response.data.count
        })

        // await axios
        //     .get(`/api/v1/leads/?page=${this.currentPage}&search=${this.query}`)
        //     .then(response => {
        //         this.leads = response.data.results
        //         console.log(response.data)

        //     // for (let i = 0; i < response.data.length; i++) {
        //     //     this.leads.push(response.data[i])

        //         if (response.data.next) {
        //             this.showNextButton = true
        //         }
        //         if (response.data.previous) {
        //             this.showPreviousButton = true
        //         }
        // })

        .catch((error) => {
          console.log(error);
        });
      // this.$store.commit('setIsLoading', false)
    },
    
    async getDrivers() {
      // this.$store.commit('setIsLoading', true)
      this.showNextButton = false;
      this.showPreviousButton = false;
      let config = {
        headers: {
          Authorization: "Bearer " + localStorage.getItem("access_token"),
          "Content-Type": "application/json",
        },
      };
      
      await axios
        .get(`/driver/`,config)
        .then((response) => {
          console.log(response.data);
          this.drivers = response.data;
          // this.num_orders = response.data.count
        })

        // await axios
        //     .get(`/api/v1/leads/?page=${this.currentPage}&search=${this.query}`)
        //     .then(response => {
        //         this.leads = response.data.results
        //         console.log(response.data)

        //     // for (let i = 0; i < response.data.length; i++) {
        //     //     this.leads.push(response.data[i])

        //         if (response.data.next) {
        //             this.showNextButton = true
        //         }
        //         if (response.data.previous) {
        //             this.showPreviousButton = true
        //         }
        // })

        .catch((error) => {
          console.log(error);
        });
      // this.$store.commit('setIsLoading', false)
    },
    async submitForm() {
      // this.$store.commit('setIsLoading', true)
      console.log("Submit Form");
      console.log(this.truck);
      // let config = {
      //   headers: {
      //   "Authorization": localStorage.getItem('Token '+'access_token') ,
      //   }
      //   }
      //   console.log(config);

      let config = {
        headers: {
          Authorization: "Bearer " + localStorage.getItem("access_token"),
          "Content-Type": "application/json",
        },
      };

      const order = {
        company: this.company,
        driver: this.driver,
        truck: this.truck,
        trailer: this.trailer,
        order_quantity: this.order_quantity,
        destination: this.destination,
        customer_id: localStorage.getItem("customer_id"),
      };
      console.log(order);
      await axios
        .post("/order/", order, config)
        .then((response) => {
          console.log(response.data);
          //  toast({
          //     message: 'The Lead is Added',
          //     type: 'is-success',
          //     dismissible: true,
          //     pauseOnHover: true,
          //     duration: 2000,
          //     position: 'bottom-right',
          // })

          this.$router.push("/");
        })
        .catch((error) => {
          console.log(error);
        });

      // this.$store.commit('setIsLoading', false)
    },
  },
};
</script>
