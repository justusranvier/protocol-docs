# Document Type `<instrumentDefinition>`

Inherits from base Document Type Contract.

## Elements and attributes

* Attribute `version`. Integer. Hard-coded to 2.0
* Element `entity`.
    * Attribute `shortname`. String.
    * Attribute `longname`. String.
    * Attribute `email`. String.
* Element `issue`.
    * Attribute `company`. String.
    * Attribute `email`. String.
    * Attribute `contractUrl`. String.
    * Attribute `type`. String. "currency" or "shares"
    * Optional attribute `federated`. Boolean. If not present, assumed
    to be false.
    * Optional attribute `instrumentDefinitionID`. Descriptor. Only
    valid if `federated` is "true".
* Optional element `currency`. Included if `type` is "currency".
    * Attribute `name`. String.
    * Attribute `tla`. String.
    * Attribute `symbol`. String.
    * Attribute `type`. String.
    * Attribute `factor`. String.
    * Attribute `decimal_power`. String.
    * Attribute `fraction`. String.
* Optional element `shares`. Included if `type` is "shares".
    * Attribute `name`. String.
    * Attribute `symbol`. String.
    * Attribute `type`. String.
    * Attribute `issuedate`. String.
    * Optional attribute `payments`. Descriptor. Included if `federated` is "true".
* Optional element `units`. Included if `federated` is "true". May occur
more than once
    * Attribute `color`. Descriptor.
    * Attribute `scale`. Integer.  Conversion factor between color
    units and contract units.

## Federation

Federated instrumentDefinitions are identified by a smart property rather than a hash, where the smart property is controlled by the issuer of the instrument definition.

Checking the validity of a federated instrumentDefinition requires access to the blockchain.

A federated instrumentDefinition is valid if the value of the smart property given by `instrumentDefinitionID` has ever contained the hash of the instrumentDefinition.

A federated instrumentDefinition is current if the most recent value of the smart property is the hash of the instrumentDefinition.

Federated instrumentDefinitions are mutable because the value of the smart property can change, but not all changes to the definition are considered valid by OT clients.

The only valid mutation of a federated instrumentDefinition is the addition of new `units` elements.

Any other change should be considered fraudulent.

### Payments

For instruments which the holders of the units can expect to receive periodic payments (dividends), the `payments` attribute specifies the instrumentDefinitionID of the currency which the holder will receive.

## Blockchains and instrumentDefinitionIDs

In certain cases (such as for `payments` for federated instrumentDefinitions) it may be necessary to refer to a blockchain using an instrumentDefinitionID.

An instrumentDefinitionID for a blockchain is created by converting the hash of the blockchain's genesis block to an Identifier. This generated instrumentDefinitionID is the canonical way by which OT will refer to the blockchain.

* Bitcoin: `otxMkLjt55ZezBNTVi8NsaJF3oT5ELWL5gTq`

## Example

```xml
<instrumentDefinition version="2.0">

<entity shortname="Satoshi"
 longname="Satoshi Nakamoto"
 email="satoshi@nakamoto.com"/>

<issue company="Swedish Coins"
 email="info@swedishcoins.net"
 contractUrl="https://swedishcoins.net/contract"
 type="currency"/>

<currency name="Bitcoins" tla="BTC" symbol="BTC" type="decimal" factor="1000"
decimal_power="3" fraction="mBTC" />

<!-- CONDITIONS -->

<condition name="audit">
  Bitcoins are audited monthly by highly trusted people.
</condition>

</instrumentDefinition>
```

## References

[AssetContract::CreateContents()](https://github.com/Open-Transactions/opentxs/blob/be111238c0feb569462b2e710e7570c00aa3d8db/src/core/AssetContract.cpp#L776)
