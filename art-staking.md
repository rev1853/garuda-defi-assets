# 📝 Staking DApp Proposal: ART & USTC Dual-Staking Mechanism

---

## 🔷 1. Overview

This proposal outlines the design and technical approach for building a **staking decentralized application (DApp)** on **Terra Classic**, featuring **dual staking mechanisms** for:

1. **CW20-based staking** of ART (Artemis Token)
2. **USTC staking**, capped by the ART staking value

Due to the lack of an active oracle on Terra Classic, two approaches are proposed for managing the valuation relationship between ART and USTC in the second staking mechanism: an **off-chain voucher model** and an **on-chain push oracle**.

---

## 🔷 2. Features Summary

| Feature                    | Description                                                              |
| -------------------------- | ------------------------------------------------------------------------ |
| CW20 Token Staking         | Users stake ART token to receive rewards and ranking                     |
| Multi-Token Rewards        | Rewards distributed in multiple tokens, via APR or fixed token/day basis |
| Rank Tier System           | Users achieve ranks based on their staked ART amount                     |
| USTC Staking Cap Mechanism | Users can stake USTC only if they have equivalent ART staked             |
| Off-Chain Voucher Option   | Uses backend-generated voucher to enforce USTC staking cap               |
| Optional Push Oracle       | On-chain price feed via backend for dynamic price validation             |

---

## 🔷 3. CW20 (ART) Staking Mechanism

* **Token Type:** CW20 (Artemis Token)
* **Function:** Standard staking mechanism.
* **User Ranking:** Based on amount staked.
* **Reward Options:**

  * Configurable per pool: either **APR-based** or **token-per-day** emissions
  * Supports multiple reward tokens

---

## 🔷 4. USTC Staking (ART-Based Cap)

### ✅ Objective

To allow users to stake USTC, **but only as much as their equivalent staked ART value**.

### 🛠 Mechanism Options

#### Option A: Voucher-Based Cap Enforcement

* **Process:**

  1. User initiates a request to stake USTC.
  2. Backend checks user’s ART stake and DEX price ratio (ART/USTC).
  3. A **signed voucher** is generated including:

     * Allowed USTC stake amount
     * Expiry timestamp
     * Signature (for smart contract verification)
  4. User submits voucher on-chain to complete USTC staking.
* **Security:** Vouchers are signed by backend and expire within a short window (e.g., 15 mins).
* **Advantages:**

  * Efficient and secure
  * No need for live oracle
  * Low gas usage
* **Challenge:** Backend availability, key security, DEX price API

#### Option B: Push Oracle 

* **Process:**

  * Backend regularly pushes price data (USTC/ART) from DEX into an on-chain oracle contract.
  * The smart contract uses this data to validate USTC staking cap.
* **Advantages:**

  * Fully on-chain, no voucher handling
* **Disadvantages:**

  * Need to provide some lunc for push gas fee. Estimation gas fee each push: 20 lunc, for 1 hour update, we need 480 lunc each day. The update interval can be adjusted if the ART token traffic is high.

---

