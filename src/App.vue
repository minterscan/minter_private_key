<template>
  <div id="app">
    <logo />
    
    <div class="field has-addons form">
      <!-- Menemonic Field -->
      <p class="control is-expanded">
        <input 
          class="input"
          type="password"
          v-model="mnemonic"
          placeholder="Mnemonic Phrase"
        >
      </p>
      <p class="control">
        <!-- Set Mnemonic Button -->
        <button 
          class="button is-outlined is-info" 
          @click="getPrivateKey()"
        >
          Set
        </button>
      </p>
    </div>

    <div class="notification private-key">
      <b-loading :is-full-page="false" :active="isLoading" :can-cancel="false" />
      {{ privateKey }}
    </div>
  </div>
</template>

<script lang="ts">
import Logo from '@/components/Logo.vue'
import { walletFromMnemonic } from 'minterjs-wallet'
import { Component, Vue } from 'vue-property-decorator'

@Component({
  components: {
    Logo
  }
})
export default class App extends Vue {
  protected mnemonic: string = ''
  protected privateKey: string = ''
  protected isLoading: boolean = false

  protected getPrivateKey() {
    this.isLoading = true

    this.$nextTick(() => {
      const wallet = walletFromMnemonic(this.mnemonic)

      this.isLoading = false
      this.privateKey = wallet.getPrivateKeyString()
    })
  }
}
</script>
