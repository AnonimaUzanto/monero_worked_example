# Monero Worked Example

CAUTION: This is a work in progress. Results have not been peer reviewed.

Note: There is one function used in this code that the author added to a fork of the pynacl library. The function `nacl.bindings.crypto_core_ed25519_from_uniform` may be added to pynacl but is not yet in an official release.


# Introduction

Is there a concrete worked example, with all intermediate steps, of Monero being received and then sent? There is now.

There are a plethora of amazing Monero resources freely available, but the author has not yet seen a resource that walks through the entire lifecycle of Monero transactions from address creation via mnemonic, receipt and transmission of transactions, to validation of a transaction prior to inclusion in a block. Curiosity is quite the driving force, and so the author was compelled to create such a resource.

The files contain within will walk through in detail receiving and sending Monero transactions using real transactions stored in the blockchain. The files present three different people: Alice (recipient and then sender of Monero), Bob (recipient of Alice's sent Monero), and Veronica (a validator ensuring the transaction is valid). The files will display and reference real transactions anyone with a full copy of the Monero database can independently view. All code is written in Python for readability and accessibility.

The worked example follows along with the book [Zero to Monero: Second Edition](https://www.getmonero.org/library/Zero-to-Monero-2-0-0.pdf) to show concretely what is occurring in each section. **As such, references to specific sections of that book will be included, wherever possible, within relevant code blocks to provide additional context.** The worked example uses Jupyter notebooks which allow the reader to run all of the included code.

The three perspectives in the worked example are summarised as follows:
* Alice
  * previously used Monero but the computer she was using crashed and she is in the process of recovering her Monero data
  * has access to her 25 word mnemonic which will be used to build her public/private view/spend keys and her Monero address
  * she is the recipient of one transaction on the blockchain
    * block id: 2871712 
      * command to print block data: `monerod print_block 2871712`
    * transaction id: [be71537a3857498b00c2458f8710d8dbc0de02018607857cc27b80afa242dc61](./worked_example/transactions/inbound/txn_be71.json) 
      * command to print transaction data: `monerod print_tx be71537a3857498b00c2458f8710d8dbc0de02018607857cc27b80afa242dc61 +json`
  * she will send one transaction to Bob using Bob's address
    * block id: 2871723
      * command to print block data: `monerod print_block 2871723`
    * transaction id: [9e2955cc46dfb5dee77b986fd318c47ec947c1a54d04b667e3f6920157aa3067](./worked_example/transactions/outbound/txn_9e29.json)
      * command to print transaction data: `monerod print_tx 9e2955cc46dfb5dee77b986fd318c47ec947c1a54d04b667e3f6920157aa3067 +json`
* Bob
  * has access to his mnemonic (and the corresponding public/private view/spend keys just like Alice)
  * he has received the above transaction in block 2871723 from Alice
* Veronica
  * she knows nothing about Alice or Bob but has to validate the transaction Alice sent to Bob is valid

This worked example will be split into sections for Alice and Veronica, and will walk through all of the calculations they can perform while displaying the actual relevant sections from the transaction data stored in the blockchain. The use and importance of public/private view/spend keys will be made apparent in the worked example.

* Alice will:
  * Convert her mnemonic to public/private view/spend keys and Monero address
  * Identify received transaction and decode all possible data
  * Generate a valid transaction to Bob
* Veronica will:
  * Show that the transaction sent from Alice to Bob can be validated without knowing anything about Alice or Bob

Each section begins with a summary of data available and relevant for that section, and ends with a summary of data available and relevant after completing the steps within the section.

This worked example draws heavily from the following four highly recommended sources:
* [Zero to Monero: Second Edition](https://www.getmonero.org/library/Zero-to-Monero-2-0-0.pdf)
* [Monero Inflation](https://moneroinflation.com/) ([Github](https://github.com/DangerousFreedom1984/monero_inflation_checker))
* [Monero Python](https://monero-python.readthedocs.io/en/latest/)
* [Mininero](https://github.com/monero-project/mininero)

**Note: Bulletproofs+ are not yet covered. To the author they appear very complex, and the author will require a significant amount of time to add them. They are planned for inclusion in the worked example.**

# Table of Contents
* [1 - Alice address calculations](./worked_example/alice/1_address_calculation.ipynb)
* Received transaction
    * [2 - Received transaction ownership calculations](./worked_example/alice/inbound_transaction/2_ownership_calculation.ipynb)
    * [3 - Received transaction spending calculations ](./worked_example/alice/inbound_transaction/3_received_data_calculations.ipynb)
    * [4 - Received transaction summary](./worked_example/alice/inbound_transaction/4_summary_of_input.ipynb)
* Sent transaction
    * [5 - Spending address calculations](./worked_example/alice/outbound_transaction/5_sending_address_calculations.ipynb)
    * [6 - Spend amount calculations](./worked_example/alice/outbound_transaction/6_spend_amount_calculations.ipynb)
    * [7 - CLSAG calculations](./worked_example/alice/outbound_transaction/7_clsag_calculations.ipynb)
* Validation
    * [8 - Amount validation](./worked_example/veronica/8_range_proof_calculation.ipynb)
    * [9 - CLSAG validation](./worked_example/veronica/9_clsag_validation.ipynb)