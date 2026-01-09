# Centrifuge Starter Kit

This repository contains sample implementations of contracts for [Centrifuge](https://github.com/centrifuge/protocol).

## `DepositRedeemFeeManager`

A sample Hub Manager that wraps `BatchRequestManager.issueShares` and `BatchRequestManager.revokeShares` to apply configurable deposit and redeem fees.

### Features

- **Deposit fee**: Increases the effective share price, so investors receive fewer shares for their deposit
- **Redeem fee**: Decreases the effective share price, so investors receive less payout for their shares
- **Configurable**: Fees can be set per pool by authorized hub managers via `setFees()`

### Usage

```solidity
// Deploy the fee manager
DepositRedeemFeeManager feeManager = new DepositRedeemFeeManager(hubRegistry, batchRequestManager);

// Register as hub manager for your pool
hub.updateHubManager(poolId, address(feeManager), true);

// Set fees (1% deposit, 2% redeem)
feeManager.setFees(poolId, d18(0.01e18), d18(0.02e18));

// Use issueShares/revokeShares through the fee manager instead of batchRequestManager
feeManager.issueShares{value: gas}(poolId, scId, assetId, epochId, pricePoolPerShare, extraGas, refund);
feeManager.revokeShares{value: gas}(poolId, scId, assetId, epochId, pricePoolPerShare, extraGas, refund);
```

### Note

This contract only implements fee accounting by adjusting prices. Withdrawing the corresponding fee assets from the holdings should be implemented separately.

## License

MIT
