# Midnight Miner

A Python-based mining bot for the Midnight Network's scavenger hunt, allowing users to automatically mine for NIGHT tokens with multiple wallets.

## How It Works

The miner operates by performing the following steps:
1.  **Wallet Setup**: It creates or loads several Cardano wallets to be used for mining. These wallets are stored in `wallets.json`.
2.  **Registration**: The bot registers the wallets with the Midnight Scavenger Mine API and agrees to the terms and conditions.
3.  **Challenge Loop**: It continuously polls the API for new mining challenges. Challenges are stored in `challenges.json` so they can be resumed later.
4.  **Mining**: When a new challenge is received, the bot uses the provided parameters to build a large proof-of-work table (ROM). It then rapidly searches for a valid nonce that solves the challenge's difficulty requirement. The core hashing logic is performed by a WebAssembly module (`ashmaize_web_bg.wasm`) for performance.
5.  **Submission**: Once a solution is found, it is submitted to the API to earn NIGHT tokens.

## Prerequisites

Before running the miner, ensure you have the following:

1.  **Python 3**: The script is written in Python.
2.  **Required Libraries**: Install the necessary Python packages using pip:
    ```bash
    pip install wasmtime requests pycardano cbor2
    ```

## Usage

You can run the miner from your terminal.

-   **Start mining**:
    This command will either load an existing wallet from `wallets/wallet1.json` or guide you through creating a new one if it doesn't exist.
    ```bash
    python miner.py
    ```

-   **Multiple workers/wallets**:
    To mine with multiple wallets, use:
    ```bash
    python miner.py --workers <number of workers>
    ```
    Each worker typically uses one CPU core. For optimal performance, avoid running more workers than your CPU has cores.

    ```bash
    python main.py --wallet-file path/to/your_wallet.json
    ```

## Exporting Wallets

To access your earned NIGHT tokens, you will need to import your wallets' signing keys (`.skey` files) into a Cardano wallet like Eternl. The `export_skeys.py` script helps with this process.

1.  **Run the export script**:
    ```bash
    python export_skeys.py
    ```
    This will create a directory named `skeys/` (if it doesn't exist) and export each wallet's signing key from `wallets.json` into a separate `.skey` file (e.g., `skeys/wallet_0.skey`).

2.  **Import into Eternl (or other Cardano wallet)**:
    *   Open your Eternl wallet.
    *   Go to `Add Wallet` -> `More` -> `CLI Signing Keys`.
    *   Import the `.skey` files generated in the `skeys/` directory.

## Disclaimer

This is an unofficial tool. Use it at your own risk. The security of your wallet is your responsibility. Ensure you keep your wallet files (`*.json`) safe and private, and never share your `.skey` files. They contain the private keys required to access any earned tokens.
