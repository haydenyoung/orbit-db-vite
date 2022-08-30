<template>
  <v-container>
    <h1>Edit Profile</h1>

    <v-form ref="form" v-model="valid" lazy-validation>
      <v-text-field
        v-model="name"
        :counter="255"
        :rules="nameRules"
        label="Name"
        required
      ></v-text-field>

      <v-btn :disabled="!valid" color="success" class="mr-4" @click="save">
        Save
      </v-btn>
    </v-form>
  </v-container>
</template>

<script type="ts">
import * as IPFS from "ipfs-core";
import OrbitDB from "orbit-db";
import EthIdentityProvider from "orbit-db-identity-provider";
import { ethers } from "ethers";

export default {
  name: "ProfileEditor",

  data: () => ({
    valid: true,
    name: "",
    nameRules: [
      (v) => !!v || "Name is required",
      (v) => (v && v.length <= 10) || "Name must be less than 255 characters",
    ],
  }),
  async mounted() {
    console.log(OrbitDB);
    const ipfs = await IPFS.create();
    const orbitdb = await OrbitDB.createInstance(ipfs);

    const provider = new ethers.providers.Web3Provider(window.ethereum);
    await provider.send("eth_requestAccounts", []);
    const wallet = provider.getSigner();
    console.log("Is Supported", OrbitDB.Identities.isSupported("ethereum"));

    const identity = OrbitDB.Identities.createIdentity({
      type: "ethereum",
      wallet
    });

    const db = await orbitdb.keyvalue('first-database', { identity: identity });

    console.log("mounted", ipfs, provider, identity);
  },
  methods: {
    async save() {
      // const ipfs = await IPFS.create();
      // const orbitdb = await OrbitDB.createInstance(ipfs);
      /*
      const provider = new ethers.providers.Web3Provider(window.ethereum);
      const wallet = provider.getSigner();
      const identity = await Identities.createIdentity({
        type: "ethereum",
        wallet,
      });

      console.log("init db", "identity", identity.id);
      const db = await orbitdb.keyvalue(
        "quadratic.profile",
        { accessController: { write: [identity.id] },
        create: true }
      );
      console.log("db ready");
      await db.load();

      await db.set("name", { value: this.name });

      console.log(db.get("name"));*/
    },
  },
};
</script>
