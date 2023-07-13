# Liquidation

Arvin Finance follows specific rules regarding liquidation to maintain the over-collateralization of its USD-pegged stablecoin, IN. Liquidation occurs when the ratio of borrower debt to collateral asset value reaches a predetermined threshold. This threshold, known as the "maximum collateral rate," signifies the point at which a debt position becomes eligible for liquidation within the Arvin Finance ecosystem.

It is important to note that Arvin Finance operates as an isolated lending market, meaning that each collateralized debt position (CDP) is evaluated independently. In cases where a borrower possesses multiple CDPs, liquidation will only occur in the CDP that meets the liquidation conditions, sparing the borrower's other borrowing positions from liquidation.

Liquidation is a permissionless process, allowing any eligible third party to engage in the liquidation procedure. When a borrower's CDP meets the liquidation criteria, a liquidator can repay the borrower's principal and interest. As a reward for their efforts, the protocol provides a portion of the borrower's collateral to the liquidator, known as the "liquidation fee." Notably, 10% of the liquidation fee is distributed to the protocol itself.

Definitions:

1. Collateral ratio: This ratio is calculated by dividing the borrowed amount of IN by the collateral value. It serves as an indicator of the lending health, with a lower collateral ratio indicating that IN funds are backed by a greater amount of collateral.
2. Maximum collateral rate: Each vault within Arvin Finance sets a specific parameter known as the maximum collateral rate. When the collateral ratio of a CDP exceeds this predetermined maximum collateral ratio, the position becomes eligible for liquidation.

Example: Let's consider an example involving Bob and his holdings:

* Bob possesses 1 ETH, with each ETH valued at $2000.
* The maximum collateral ratio for the ETH vault is set at 80%.
* Bob deposits 1 ETH as collateral and borrows 800 IN, resulting in a current collateral ratio of 800/2000 = 40%.

If the price of ETH drops to $1000, Bob's collateral ratio would be 800/1000 = 80%, triggering the liquidation condition. Alice, acting as a liquidator, identifies this opportunity and pays off Bob's debt of 800 IN using 800 USDC. The liquidation fee is calculated as 1 ETH \* 6% = 0.06 ETH. Bob would have 0.14 ETH remaining in his vault, and he would not have any further debt obligations.
