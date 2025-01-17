---
description: >-
  Helper contract for the GoodReserveCDai. It serves as a dynamic reserve ratio
  market maker.
---

# GoodMarketMaker

### Events

#### BalancesUpdated

Emits when a change has occurred in a reserve balance, i.e. buy / sell will change the balance.

| Parameter name | Annotation                            |
| -------------- | ------------------------------------- |
| caller         | The account who initiated the action. |
| reserveToken   | The address of the reserve token.     |
| amount         | The incoming amount.                  |
| returnAmount   | The return value.                     |
| totalSupply    | The updated total supply.             |
| reserveBalance | The updated reserve balance.          |

```
event BalancesUpdated(
    address indexed caller,
    address indexed reserveToken,
    uint256 amount,
    uint256 returnAmount,
    uint256 totalSupply,
    uint256 reserveBalance
);
```

#### ReserveRatioUpdated

Emits when the ratio changed. The caller should be the Avatar by definition.

| Parameter name | Annotation                 |
| -------------- | -------------------------- |
| caller         | The address of the staker. |
| nom            | Nominator of the ratio.    |
| denom          | Denominator of the ratio.  |

```
event ReserveRatioUpdated(address indexed caller, uint256 nom, uint256 denom);
```

### calculateNewReserveRatio

Calculates how much to decrease the reserve ratio for `_token` by the `reserveRatioDailyExpansion`.

| Parameter name | Annotation                                            |
| -------------- | ----------------------------------------------------- |
| \_token        | The reserve token to calculate the reserve ratio for. |

Returns: the new reserve ratio.

```
function calculateNewReserveRatio(ERC20 _token) public view returns (uint32);
```

### buyReturn

Calculates the buy return in G$ according to the given `_tokenAmount`.

| Parameter name | Annotation                               |
| -------------- | ---------------------------------------- |
| \_token        | The reserve token buying with.           |
| \_tokenAmount  | The amount of reserve token buying with. |

Returns: A number of G$ that should be given in exchange as calculated by the bonding curve.

```
function buyReturn(ERC20 _token, uint256 _tokenAmount);
```

### sellReturn

Calculates the sell return in `_token` according to the given `_gdAmount`.

| Parameter name | Annotation                               |
| -------------- | ---------------------------------------- |
| \_token        | The reserve token buying with.           |
| \_gdAmount     | The amount of reserve token buying with. |

Returns: A number of tokens that should be given in exchange as calculated by the bonding curve.

```
function sellReturn(ERC20 _token, uint256 _gdAmount);
```

### buy

Updates the `_token` bonding curve params. Emits `BalancesUpdated` with the new reserve token information.

| Parameter name | Annotation                               |
| -------------- | ---------------------------------------- |
| \_token        | The reserve token buying with.           |
| \_tokenAmount  | The amount of reserve token buying with. |

Returns: A number of G$ that will be given in exchange as calculated by the bonding curve.

```
function buy(ERC20 _token, uint256 _tokenAmount);
```

### sellWithContribution

Calculates the sell return with contribution in `_token` and update the bonding curve params. Emits `BalancesUpdated` with the new reserve token information.

| Parameter name         | Annotation                                                             |
| ---------------------- | ---------------------------------------------------------------------- |
| \_token                | The desired reserve token to have.                                     |
| \_gdAmount             | The amount of G$ that are sold.                                        |
| \_contributionGdAmount | The number of G$ tokens that will not be traded for the reserve token. |

Returns: A number of tokens that will be given in exchange as calculated by the bonding curve.

```
function sellWithContribution(
	ERC20 _token,
	uint256 _gdAmount,
	uint256 _contributionGdAmount
) public returns (uint256);
```
