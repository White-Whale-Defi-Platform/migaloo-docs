# Bridge Assets

{% hint style="danger" %}
When bridging IBC assets must ALWAYS  route through their home chain. For example bridging WHALE from Osmosis to Terra would Require first IBCing WHALE to Migaloo first, then IBC to Terra. Bridges such as TFM and IBC FUN _should_ automatically route correctly for you, but manual IBC sends using wallets will need routing manually.
{% endhint %}

### Station Wallet:

{% embed url="https://docs.station.money/using/send" %}

### LEAP Wallet

{% embed url="https://leapwallet.notion.site/Make-IBC-transfer-6a4dcb76ad8a4d1baee6a969fbfc64fb" %}

### Keplr Wallet:

{% embed url="https://help.keplr.app/articles/ibc-transfers" %}

### TFM:

{% embed url="https://docs.tfm.com/quick-start#ibc-transfer" %}

### IBC FUN

{% embed url="https://api-docs.skip.money/docs/getting-started" %}

## IBC Channels

| Network   | Channel   |
| --------- | --------- |
| Terra 2   | 0 <> 86   |
| Juno      | 1 <> 210  |
| Injective | 3 <> 102  |
| Secret    | 4 <> 57   |
| Osmosis   | 5 <> 642  |
| Comdex    | 12 <> 63  |
| Chihuahua | 10 <> 39  |
| Kujira    | 8 <> 58   |
| Carbon    | 14 <> 24  |
| Neutron   | 27 <> 57  |
| Umee      | 22 <> 57  |
| Kava      | 54 <> 126 |
| Axelar    | 53 <> 121 |
| Lunc      | 59 <> 84  |
| Sei       | 57 <> 14  |
| Noble     | 60 <> 14  |

{% hint style="warning" %}
First number is the source and second is destination. Example: Migaloo 0 <-> 86 Terra. On Migaloo, you go from channel 0. Source is always Migaloo first
{% endhint %}
