### Add ZavaX Validator Sequence Notes
This diagram shows the straightforward sequence of adding a subnet validator to the Avalanche primary network. It does not involve any ZavaX components. 

The only unusual thing to note is that the *memo* field for the *add validator* P-chain transaction is used to store a json-formatted string that sets the fee schedule that this validator's ZavaX Agent will use when it bridges transactions, once it becomes a guardian. This fee schedule includes the amount of *seed* AVAX provided to ZEC Owners when they bridge from Zcash to Avalanche.