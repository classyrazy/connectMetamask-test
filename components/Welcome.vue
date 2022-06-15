<template>
    <div>
        <h1 class="hero-header-text">Discover, Connect, Interact with your Ethereum Account through Metamask</h1>
        <div class="btn-con" v-if="isMetaMaskInstalled">
            <p-button :icon="MetamaskSvg" class="hero-btn" @click="handleConnectMetamask" v-if="!account"
                :loading="loading"><span>{{ loading ? 'Connecting' : 'Connect' }}</span> to
                metamask</p-button>
        </div>
        <div class="get-started-con" v-else>
            <h3 class="hero-header-sub-text">To get started install Metamask</h3>
            <a class="btn-con" target="_blank"
                href="https://chrome.google.com/webstore/detail/metamask/nkbihfbeogaeaoehlefnkodbefgpgknn">
                <p-button :icon="MetamaskSvg" class="hero-btn">Install metamask
                </p-button>
            </a>
        </div>
        <div class="account-details btn-con" v-if="account">
            <input type="text" v-model="account" placeholder="Your account address" disabled class="copy-input" />
            <div class="">
                <input type="text" v-model="balance" v-if="balance" placeholder="Your account address" disabled
                    class="copy-input" />
                <p v-else>Loading balance</p>
            </div>
        </div>
        <h2 v-if="errorMsg">
            {{ errorMsg }}

        </h2>
    </div>
</template>

<script setup lang="ts">
import PButton from './forms/p-button.vue'
import MetamaskSvg from './svgs/metamask-svg.vue'
import { ethers } from 'ethers';
let loading = ref(false)
let isMetaMaskInstalled = ref(false)
let account = ref(null)
let errorMsg = ref(null)
let balance = ref(null)
// let ethereum = window.ethereum
const ethereum = window.ethereum;

const checkMetaMaskInstalled = () => {
    if (window.ethereum && window.ethereum.isMetaMask) {
        isMetaMaskInstalled.value = true
    }

}
const getbalance = async (address: string) => {
    try {
        loading.value = true
        const balanceReq = await ethereum.request({
            method: "eth_getBalance",
            params: [address, "latest"]
        });

        balance.value = ethers.utils.formatEther(balanceReq)
        if (balance) {
            loading.value = false
        }
    } catch (error) {
        console.log(error);
    }
};
async function handleConnectMetamask() {
    try {
        loading.value = true
        const accounts = await ethereum.request({ method: 'eth_requestAccounts' });
        account.value = accounts[0];
        if (account) {
            loading.value = false
            getbalance(account.value)
        }
    } catch (error) {
        loading.value = false
        errorMsg.value = error.message

    }
}
onMounted(() => {
    checkMetaMaskInstalled()
    console.log(account.value)
})

</script>

<style scoped>
.hero-header-text {
    font-size: 2.5rem;
    font-weight: bold;
    color: #fff;
    text-align: center;
    /* margin-top: 20px; */
    font-family: var(--font-monts);

}

.hero-header-sub-text {
    font-size: 1.5rem;
    font-weight: semi-bold;
    color: rgba(255, 255, 255, 0.631);
    text-align: center;
    /* margin-top: 20px; */
    font-family: var(--font-monts);

}

.get-started-con {
    margin-top: 70px;
}

.btn-con {
    display: flex;
    justify-content: center;
    margin-top: 20px;
}

.hero-btn {
    border-radius: 10px;
}

.btn-icon {
    width: 0px;
    height: 20px;
    margin-left: 10px;
}

@media only screen and (min-width: 600px) and (max-width: 899px) {
    .hero-header-text {
        font-size: 1.5rem;
    }

    .hero-header-sub-text {
        font-size: 1.2rem;
    }
}
@media (max-width: 599px) {
    .hero-header-text {
        font-size: 1.2rem;
    }

    .hero-header-sub-text {
        font-size: 1rem;
    }
}

</style>