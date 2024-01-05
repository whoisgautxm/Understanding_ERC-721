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


    



}
```