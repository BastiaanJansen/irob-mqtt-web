<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <h1>RFID Dashboard</h1>
    </v-app-bar>

    <v-main>
      <v-container>
        <v-form class="form" align="center">
          <v-row>
            <v-text-field
              v-model="pass"
              label="Pass ID"
              required
            ></v-text-field>

            <v-spacer></v-spacer>

            <v-btn class="mt-4" color="blue" @click="createPass(pass)">
              Create pass
            </v-btn>
          </v-row>
        </v-form>

        <v-simple-table>
          <template v-slot:default>
            <thead>
              <tr>
                <th class="text-left">ID</th>
                <th class="text-left">Pass</th>
                <th class="text-left">Options</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="pass in passes" :key="pass.id">
                <td>{{ pass.id }}</td>
                <td>{{ pass.pass }}</td>
                <td>
                  <v-btn color="error" @click="deletePass(pass)">
                    Delete
                  </v-btn>
                </td>
              </tr>
            </tbody>
          </template>
        </v-simple-table>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import axios from "axios";

export default {
  name: "App",

  components: {},

  data: () => ({
    passes: [],
    url: "http://s1121859:3000",
    pass: "",
  }),
  async mounted() {
    this.fetchPasses();
  },
  methods: {
    async fetchPasses() {
      const { data: passes } = await axios.get(this.url + "/passes");
      this.passes = passes;
    },
    async createPass(passId) {
      await axios.post(this.url + "/passes", {
        pass: passId,
      });
      this.fetchPasses();
    },
    async deletePass(pass) {
        await axios.delete(this.url + "/passes/" + pass.id);
        this.fetchPasses();
    } 
  },
};
</script>
