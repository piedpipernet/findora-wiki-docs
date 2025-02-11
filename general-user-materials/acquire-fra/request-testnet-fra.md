# Request (Testnet) FRA

To test the Findora suite of applications or get started with building on the Findora network, you can request to receive testnet tokens via our `Discord` bot: `FindoraBot`.

## Request on Findora EVM Chain

Below are the steps to request Anvil testnet FRA tokens on the EVM Chain.

Step 1: Go to [Findora Discord](https://discord.com/invite/findora)

Step 2: Go to `#faucet` channel on Findora's Discord

Step 3: FindoraBot will automatically detect commands requesting Testnet FRA EVM tokens. Specify the following command to send a request:

Bot Request Format:

```bash
!evm <0x wallet address>
```

### Example:

```bash
!evm 0xe11F5c3e132B8B52203aFaAd849434373D48E4c1
```

### Forge Testnet

```
!evm forge <0x wallet address>
```

{% hint style="info" %}
Note: You can only ask for FRA tokens once every 24 hours, so make sure your receiving wallet address is correct.
{% endhint %}

## Request on Findora Native Chain[​](https://wiki.findora.org/docs/evm\_guides/get\_fra/faucet#request-on-findora-native-chain) <a href="#request-on-findora-native-chain" id="request-on-findora-native-chain"></a>

Below are the steps to request Anvil testnet FRA tokens on the UTXO Chain.

Step 1: Go to [Findora Discord](https://discord.com/invite/findora)

Step 2: Go to `#faucet` channel on Findora's Discord

Step 3: FindoraBot will automatically detect commands requesting Testnet FRA EVM tokens. Specify the following command to send a request:

Bot Request Format:

```bash
!faucet-<environment> <wallet address> <Email>
```

### Examples:

**Anvil Testnet (Staking)**

```bash
!faucet-anvil fra19rtfg2g58x6jxxxxxxxxxxxxxxxxx example@gmail.com
```

**Forge Testnet**

```
!faucet-forge fra19rtfg2g58x6jxxxxxxxxxxxxxxxxx example@gmail.com
```

#### Check FRA UTXO Balance[​](https://wiki.findora.org/docs/evm\_guides/get\_fra/faucet#check-fra-utxo-balance) <a href="#check-fra-utxo-balance" id="check-fra-utxo-balance"></a>

You will receive a message from the FindoraBot with the link to the transaction on the [Findora Native block explorer for Anvil](https://prod-testnet.findorascan.io/). You can input your fra address in the block explorer to check the transaction history.

Alternatively, you can also check your FRA balance on the Findora Native Chain using Wallet application by connecting to Anvil testnet. Go to Settings > Manage to configure and switch to Anvil Network.

{% hint style="info" %}
Note: You can only ask for FRA tokens once every 24 hours, so make sure your receiving wallet address is correct.
{% endhint %}
