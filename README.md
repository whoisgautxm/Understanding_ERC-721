# Understanding_ERC-721

## What is ERC-721?
ERC-721 is a standard interface for non-fungible tokens, meaning that each token is unique and not interchangeable. This standard allows for the implementation of a standard API for NFTs within smart contracts. This standard is used in popular projects such as CryptoKitties, Decentraland, and Gods Unchained.

## What is a Non-Fungible Token?
A non-fungible token is a unique and non-interchangeable unit of data stored on a blockchain. This is in contrast to a fungible token, which is a unit of data that is interchangeable with other units of data. For example, a fungible token could be a unit of currency, such as a dollar bill. One dollar bill is interchangeable with another dollar bill. A non-fungible token could be a concert ticket. Each concert ticket is unique and not interchangeable with other concert tickets.

## ERC-721 Contract
```
pragma solidity ^0.5.0;
/// @title ERC-721 Non-Fungible Token Standard
/// @dev See https://eips.ethereum.org/EIPS/eip-721
///  Note: the ERC-165 identifier for this interface is 0x80ac58cd.

interface ERC721 /* is ERC165 */ {
    // @dev This emits when ownership of any NFT changes by any mechanism.
   ///_from: This parameter represents the address from which the NFT   is being transferred. When a token is initially created, _from is 0 since it doesn't have a prior owner. When an existing token is transferred, _from will represent the address of the token's previous owner.
   ///_to: This parameter signifies the address to which the NFT is being transferred. After a successful transfer, this address becomes the new owner of the NFT.
   ///Ownership Change Tracking: The Transfer event is crucial for tracking the movement of NFTs. By emitting this event, it allows external systems, applications, or contracts to monitor and react to changes in ownership.

    event Transfer(address indexed _from, address indexed _to, uint256 indexed _tokenId);


    ///_owner: This parameter represents the address that currently owns the NFT for which approval is being set or changed.
    ///_approved: This parameter signifies the address that is being granted approval to manage or take specific actions on the NFT. This address can transfer the token on behalf of the owner or perform certain actions depending on the implementation.
    ///This event signals that a particular address (_approved) now has permission from the token's current owner (_owner) to perform certain operations on the NFT represented by _tokenId. Typically, this permission allows the approved address to transfer the token on behalf of the owner.

    event Approval(address indexed _owner, address indexed _approved, uint256 indexed _tokenId);

    ///_operator: This parameter signifies the address for which the approval status across all tokens of _owner is being changed.
    ///_approved: This parameter is a boolean value indicating whether the _operator is being approved (true) or disapproved (false) to manage all tokens owned by _owner.
    ///Operator Authorization Change: The ApprovalForAll event is emitted when the setApprovalForAll function is called. It signifies a change in the operator's approval status to manage all tokens owned by a specific address.
    ///Global Operator Approval: This event allows an owner to grant or revoke permission for a specific address (_operator) to manage all of their tokens. When approved, the designated operator can perform operations on all tokens owned by that address without needing individual approvals.


    event ApprovalForAll(address indexed _owner, address indexed _operator, bool _approved);


    ///@notice Count all NFTs assigned to an owner
    ///_owner: This parameter represents the address for which you want to query the number of NFTs owned.
    ///returns (uint256): The function returns an unsigned integer (uint256), which signifies the count of NFTs owned by the specified _owner
    ///Read-Only Function: It is a view function, meaning it doesn't modify the blockchain state and can be executed without incurring any gas fees. It only reads data from the blockchain.
    ///Use in User Interfaces: DApps, wallets, and other applications can utilize balanceOf to display the number of owned tokens for a user. This information is crucial for user interfaces to represent the user's token holdings accurately.

    function balanceOf(address _owner) external view returns (uint256);


    /// @notice Find the owner of an NFT
    ///_tokenId: This parameter represents the unique identifier of the NFT for which you want to determine the current owner.
    ///returns (address): The function returns an Ethereum address (address), which represents the current owner of the specified _tokenId.
    ///Essential for Verification: It's crucial for users, applications, or contracts to verify the ownership of an NFT, especially when executing transactions or enforcing access rights based on ownership.

    function ownerOf(uint256 _tokenId) external view returns (address);


    ///Safe Transfer: The safeTransferFrom function is designed to securely transfer ownership of an NFT. It ensures that the receiving address _to is capable of receiving NFTs before the transfer
    ///Checks before Transfer: When executing the transfer, this function checks if _from is the current owner, if _to is a valid address (not zero), and if _tokenId represents a valid NFT. Additionally, it             verifies if _to is a smart contract by checking its code size.
    ///Recipient Contract Validation: If the recipient address _to is a smart contract, it verifies if the contract supports the ERC721TokenReceiver interface by calling the onERC721Received function 
        in that contract. If the recipient contract doesn't support this interface or doesn't implement the onERC721Received function correctly, the transfer will fail.
    ///Handling Additional Data: The version of the function that includes bytes data parameter allows additional data to be passed, which can be interpreted by the recipient contract's onERC721Received function         if needed.

    function safeTransferFrom(address _from, address _to, uint256 _tokenId, bytes calldata data) external payable;

    function safeTransferFrom(address _from, address _to, uint256 _tokenId) external payable;


    /// @notice Transfer ownership of an NFT -- THE CALLER IS RESPONSIBLE
    ///_from: The address representing the current owner of the NFT.
    ///_to: The address to which the NFT is being transferred.
    ///_tokenId: The unique identifier of the NFT being transferred.
    ///Ownership Transfer: The primary purpose of transferFrom is to transfer the ownership of an NFT from one address (_from) to another address (_to).
    ///Allowance and Approval: For this function to succeed, the caller (msg.sender) needs to be approved by the current owner (_from)
     to perform  the transfer. It also requires the allowance or approval from _from to the caller to handle the specified token (_tokenId).


    function transferFrom(address _from, address _to, uint256 _tokenId) external payable;


    /// @notice Change or reaffirm the approved address for an NFT
    ///_approved: The address that will be granted permission to manage or transfer the NFT.
    ///_tokenId: The unique identifier of the NFT for which permission is being granted.
    ///Authorization Mechanism: The primary purpose of the approve function is to grant approval to a specific address (_approved) to handle a particular NFT (_tokenId).
    ///Resetting Approvals: To remove approval, the owner can call approve again with the _approved address set to zero (address(0)), effectively revoking permission.


    function approve(address _approved, uint256 _tokenId) external payable;


    /// @notice Enable or disable approval for a third party ("operator") to manage
    ///_operator: The address being granted or revoked approval to manage all tokens of the caller.
    ///_approved: A boolean indicating whether _operator is being approved (true) or revoked (false) for managing all tokens.
    ///Operator Authorization: The primary purpose of setApprovalForAll is to authorize or revoke an address (_operator) to manage all tokens owned by the caller of the function.


    function setApprovalForAll(address _operator, bool _approved) external;


    ///_owner: The address of the owner whose tokens are being managed.
    ///_operator: The address that is being checked for authorization to manage the tokens of _owner.
    ///returns (bool): The function returns a boolean value indicating whether _operator is authorized to manage all tokens owned by _owner.
    ///Operator Verification: DApps or smart contracts can use isApprovedForAll to verify if a particular operator has the approval to manage all tokens of a specific address.

    


    function isApprovedForAll(address _owner, address _operator) external view returns (bool);


    /// @notice Get the approved address for a single NFT
    ///_tokenId: The unique identifier of the NFT for which the approved address is being queried.
    ///returns (address): The function returns an Ethereum address (address), which represents the approved address for the specified _tokenId.
    ///Approval Query: The primary purpose of getApproved is to retrieve the address that has been explicitly approved to manage or transfer a specific token.

    function getApproved(uint256 _tokenId) external view returns (address);


interface ERC165{
    ///The ERC-165 interface, often referred to as "Standard Interface Detection," provides a standard way for smart contracts to check whether a contract implements specific interfaces or
     functionalities. This interface is not specific to ERC-721 but is a more generalized standard applicable across various Ethereum standards
    ///interfaceID: A 4-byte identifier representing the interface being queried.
    ///returns (bool): The function returns a boolean value indicating whether the queried interface is supported by the contract.
    ///Functionality Checks: Contracts can use supportsInterface to verify if another contract or address implements a specific interface or functionality before interacting with it.
    ///Standardized Identification: Interfaces are identified by their unique 4-byte identifier, commonly represented by the first four bytes of the keccak256 hash of the interface name or function signature.

    function supportsInterface(bytes4 interfaceID) external view returns (bool);
}


interface ERC721TokenReceiver{
    ///The ERC721TokenReceiver interface is a standard interface in the ERC-721 token standard that defines a function onERC721Received used by the receiving contract when it needs to receive a Non-Fungible Token (NFT) through the safeTransferFrom function
    ///_operator: The address that called the safeTransferFrom function.
    ///_from: The address from which the NFT is being transferred.
    ///_tokenId: The unique identifier of the NFT being transferred.
    ///_data: Additional data with no specified format.
    ///returns (bytes4): The function returns a bytes4 value 0x150b7a02 if the receiving contract accepts the transfer. If the receiving contract doesn't accept the transfer, it must throw an error.

    function onERC721Received(address _operator, address _from, uint256 _tokenId, bytes calldata _data) external returns(bytes4);}

}
```

## Difference between Event Transfer and Function Safe-transfer:
##### Purpose:
The safeTransferFrom function is a method used to initiate and ensure a secure transfer of ownership, performing checks to prevent unintended or unsafe transfers. In contrast, the Transfer event is a notification mechanism that signals whenever ownership changes, regardless of the method used.

##### Implementation:
 The safeTransferFrom function is part of the contract's functionality, executed when a transfer is intended, while the Transfer event is emitted internally within the contract logic to signal ownership changes.

## Difference between Function SafeTransferFrom and Function TransferFrom:
- 'transferFrom' is less secure compared to safeTransferFrom as it doesn’t check if the recipient is capable of receiving tokens, potentially resulting in lost tokens if transferred to an incompatible address.
- Unlike safeTransferFrom, transferFrom doesn't require the recipient address to support the ERC721TokenReceiver interface or have a specific onERC721Received function.


