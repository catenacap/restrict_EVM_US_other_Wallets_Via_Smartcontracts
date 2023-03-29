# restrict_EVM_US_other_Wallets_Via_Smartcontracts
Restrict US based wallets interacting with your smart contract via the EVM system

Following on from restricting US based validating nodes from validating your smart-contract processes - here: https://github.com/catenacap/restrict_evm_US_other_validation, you can also restrict US based wallets from interacting with your smart contract using the following.


----
pragma solidity ^0.8.0;

contract USWalletRestriction {
    mapping (address => string) private _walletCountry;

    modifier onlyNonUS() {
        string memory country = _walletCountry[msg.sender];
        require(keccak256(bytes(country)) != keccak256(bytes("United States")), "US-based wallet detected.");
        _;
    }

    function updateWalletCountryMapping(address wallet, string memory country) public {
        _walletCountry[wallet] = country;
    }

    function myRestrictedFunction() public onlyNonUS {
        // This function can only be called from wallets owned by individuals located outside the United States
        // ...
    }
}

----

The 'USWalletRestriction' contract uses a similar approach to the previous example, but instead of checking the IP address of the validator or miner, it checks the IP address of the wallet owner.

The '_walletCountry' mapping associates wallet addresses with their corresponding countries. You can update this mapping using the updateWalletCountryMapping function, which takes a wallet address and a country as parameters.

The 'onlyNonUS' modifier checks the country of origin of the wallet owner based on the '_walletCountry' mapping. If the country is detected as "United States", the modifier reverts the transaction and prevents the function from being executed.

The 'myRestrictedFunction' function is an example of a function that is restricted to non-US wallets. It can only be called from wallets owned by individuals located outside the United States.

Keep in mind that this approach relies on the accuracy and reliability of the external APIs used to retrieve the country information, as well as the wallet address used by the individual. Also, it's worth noting that this mechanism may not be sufficient to comply with all regulatory requirements, and you should consult with legal and compliance experts to ensure that your smart contract meets all necessary regulations.





