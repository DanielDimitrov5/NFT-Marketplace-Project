# NFT Marketplace 
 ### How it wotks?
  This is a platform that enables users to register NFT collections, sell or buy NFTs, and make offers on non-listed items. Here's how the marketplace works:
 
 1. Users can register their NFT collections with the Marketplace by using the `addCollection` function. This allows the Marketplace to track and manage the collections. In the context of this Marketplace a collection is considered a single ERC721 contract.
 2. Users who own NFTs, can add their NFTs to the Marketplace using the `addItem` function. They specify the collection ID and the token ID of the NFT they want to add. This ensures that only NFTs from registered collections can be added to the marketplace.
 3. Sellers can list their NFTs for sale by using the `listItem` function. They provide the item ID and set the price for the listed item. Only NFTs that have been previously added to the marketplace can be listed for sale.
 4. Buyers can purchase NFTs that are listed for sale by using the `buyItem` function. They specify the item ID of the NFT they want to buy and send the required payment. Upon successful purchase, the NFT is transferred from the seller to the buyer, and the seller receives the payment. A fee is also charged by the marketplace contract.
 5. Buyers can make offers on non-listed items by using the `placeOffer` function. They provide the item ID and the price they are willing to pay for the NFT. Similar to listing, the item must have been previously added to the marketplace but not listed for sale!
 6. Sellers have the option to accept offers placed by buyers using the `acceptOffer` function. They specify the item ID and the address of the offerer whose offer they want to accept.
 7. Claiming Items: Once a seller accepts an offer, the buyer can claim the item using the `claimItem` function. This transfers the NFT from the seller to the buyer and initiates the payment to the seller.
 8. The owner of the marketplace contract can withdraw the accumulated fees from the contract using the `withdraw` function. This allows the owner to collect the earnings generated from the marketplace operations.
 
## User stories

- **User:** Account willing to register NFT collections, sell or buy NFT.
- **Seller:** User who can add and list his NFTs or accept offers for his non-listed items on the Marketplace.
- **Buyer:** User who can buy listed items or place offers for other users' non-listed items.
- **Marketplace:** Smart contract which handles operations like adding, listing, buying, etc.
- **NFT:** ERC721 Smart contract which is considered a **collection** in the context of this project.
- **Item:** Structure on the Marketplace that represents NFTs with price and other useful properies.
- **Offer:** Structure that represents offer for non-listed item.
- **Owner:** Owner of the contract. He can withdraw the accumulated money from the Marketplace contract.

![User stories](/readme/UserStories.drawio.png)

## Architecture diagram
![Architecture diagram](/readme/Architecture.drawio.png)

| Function      | Parameters                                | Visibility & Modifiers | Description                                                                                                                                |
| ------------- | ----------------------------------------- | ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| addCollection | IERC721  _nftCollection                   | external               | User registers collection to the Marketplace                                                                                               |
| addItem       | uint256 \_collectionId, uitn256 \_tokenId | external               | The user adds NFT to the Marketplace. Only from registered collections!                                                                    |
| listItem      | uint256 \_itemId, uint256 \_price         | external               | Seller list NFT item for sale. Only if the item is added to the Marketplace!                                                               |
| buyItem       | uint256 \_itemId                          | external, payable      | The buyer buys the listed item, transfers the NFT to the buyer,  pays to the seller, pays fee to the contract. Only if the item is listed! |
| placeOffer    | uint256 \_itemId, uint256 \_price         | external               | Buyer places offer for non-listed item. Only if the item is added to the Marketplace!                                                      |
| acceptOffer   | uint256 \_itemId, address offerer         | external               | The seller accepts a placed offer.                                                                                                         |
| claimItem     | uint256 \_itemId                          | external, payable      | The buyer claims his item if the NFT owner has accepted his offer. It transfers the NFT to the buyer and pays the seller.                  |
| withdraw      | \-                                        | external, OnlyOwner    | The owner withdraws the accumulated fee from the contract.                                                                                 |

### Useablity specifics

1. After executing `listItem` function the Marketplace should be allowed to spend the NFT via his `approve` function.
2. After executing 'acceptOffer' function the Marketplace should be allowed to spend the NFT via his `approve` function.

## Events

| Events             | Action                                         | Parameters                                                                                                                                                                            |
| ------------------ | ---------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| LogCollectionAdded | A Collection has been added to the Marketplace | `uint256 id, IERC721 indexed nftCollection`                                                                                                                                            |
| LogItemAdded       | An Item has been added to the Marketplace      | `uint256 id,` `IERC721 indexed nftContract,` `uint256 tokenId,` `address indexed owner`                                                              |
| LogItemListed      | An Item has been listed to the Marketplace     | `uint256 id,` `IERC721 indexed nftContract,` `uint256 tokenId,` `address indexed seller,` `uint256 price`                                   |
| LogItemSold        | An Item has been sold                          | `uint256 id,` `IERC721 indexed nftContract,` `uint256 tokenId,` `address indexed seller,` `address indexed buyer,` `uint256 price` |
| LogOfferPlaced     | An Offer has been placed                       | `uint256 id,` `IERC721 indexed nftContract,` `uint256 tokenId,` `address indexed buyer,` `uint256 price`                                    |
| LogOfferAccepted   | An Offer has been accepted                     | `uint256 indexed id,` `address indexed offerer`                                                                                                                        |
| LogItemClaimed     | An Item has been claimed                       | `uint256 indexed id,` `address indexed claimer`                                                                                                                        |

