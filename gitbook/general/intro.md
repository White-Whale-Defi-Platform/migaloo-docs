# Introduction

Migaloo is a permission-less, open source network for decentralized interoperable applications.

- First Native Token Factory
- [Skip MEV](https://skip.money/) as Default

Token: WHALE  
ChainID: migaloo-1  
Chain Logo: https://whitewhale.money/migaloo.svg  
Chain Logo Alt: https://whitewhale.money/migaloo_alt.svg  
Token Logo: https://whitewhale.money/logo.svg

Keplr config:

    {
        "chainId": "migaloo-1",
        "chainName": "Migaloo",
        "rpc": "https://rpc.migaloo.silknodes.io",
        "rest": "https://api.migaloo.silknodes.io",
        "bip44": {
            "coinType": 118
        },
        "coinType": 118,
        "bech32Config": {
            "bech32PrefixAccAddr": "migaloo",
            "bech32PrefixAccPub": "migaloopub",
            "bech32PrefixValAddr": "migaloovaloper",
            "bech32PrefixValPub": "migaloovaloperpub",
            "bech32PrefixConsAddr": "migaloovalcons",
            "bech32PrefixConsPub": "migaloovalconspub"
        },
        "currencies": [
            {
                "coinDenom": "WHALE",
                "coinMinimalDenom": "uwhale",
                "coinDecimals": 6,
                "coinGeckoId": "unknown"
            }
        ],
        "feeCurrencies": [
            {
                "coinDenom": "WHALE",
                "coinMinimalDenom": "uwhale",
                "coinDecimals": 6,
                "coinGeckoId": "unknown"
            }
        ],
        "stakeCurrency": {
            "coinDenom": "WHALE",
            "coinMinimalDenom": "uwhale",
            "coinDecimals": 6,
            "coinGeckoId": "unknown"
        },
        "gasPriceStep": {
            "low": 0.01,
            "average": 0.025,
            "high": 0.03
        },
        "features": []
    }
