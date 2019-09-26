# Activate SnowGem Masternodes

1. Download [Modern Wallet](https://snowgem.org/resources/modern-wallet) and install it. When asked if you plan to install masternodes answer **Yes**.

2. Wait for the chain to be downloaded and synced.

3. Create a new transparent address (**Addresses** -> **Addresses** -> **Actions** -> **New Address**). This will be your collateral address (where you will receive the masternode rewards).

4. Transfer exactly 10000 XSG to collateral address. Wait at least 3 confirmations.

5. Go to **Masternodes** -> **Masternodes Setup**. Click **Generate Masternode Data** button. A line will be created at the bottom at the page with the **Private Key**, **Transaction ID** and **Index**. Complete the required fields including the additional **Alias Name** (How you like your Masternode to be called) and the **VPS IP** and after press **Setup**.

6. Now setup the node on the VPS side with [Unit](Setup.md).

7. After daemon have started on VPS side return to Modern Wallet on local PC. Go to **Masternodes** -> **Masternodes List**, Select your new Masternode and click **Actions** -> **Start**

_If you have any questions please check [FAQ](https://snowgem.org/faq), contact us on [Discord](https://discord.gg/zumGnbg) or [Telegram](https://t.me/snowgemofficial)._